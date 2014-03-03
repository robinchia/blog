---
layout: post
title: "SQL解析(Jsqlparser)"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - SQL_Java
--- 

# SQL解析(Jsqlparser)

前段时间主要研究了一下SQL语句的解析，主要的几个开源产品试用了一下。本文大概总结一下个人体会。
首先是ZQL，ZQL有个比较突出的优点是使用起来比较简单，基本上属于拿上手就可以用的。但支持的功能有限，selectItem可以是简单的表达式，但不支持查询作为一个selectItem，对于其他的子查询同样也不支持。
最终我选择使用了Jsqlparser，主要原因有两点：
1）功能强大，基本上能够覆盖所有的SQL语法（没有研究是不是包含了数据库特殊的关键字，如LIMIT ），包含UNION,GROUP BY,HAVING,ORDER BY,JOIN,SUB JOIN,SUB SELECT,FUNCTION等。支持SQL深层嵌套。
2）本身设计不错，使用起来感觉很灵活。Jsqlparser对于SQL的遍历采用了VISITOR模式可以很方便的遍历SQL语句。
下面主要介绍一下Jsqlparser在我的项目中的应用情况。
背景：通过SQL语句的解析，从而增强SQL语句增加特定的查询过滤条件（如：状态过滤、权限过滤等）
1）解析SQL
2）根据SQL中涉及的表增强SQL
3）重新生成ORACLE数据库的SQL
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. CCJSqlParserManager parserManager = new CCJSqlParserManager();  
1. try {  
1.     Select select = (Select) parserManager.parse(new StringReader(sql)); //解析SQL语句  
1.     SelectBody body = select.getSelectBody();   
1.     VisitContext vc = new VisitContext(filterContext, params);  
1.     vc.setTableFilterFactory(tableFilterFactory);//表的字段过滤  
1.     body.accept(new SelectVisitorImpl(vc)); //访问SQL并根据SQL中涉及的表来增强SQL  
1.     ExpressionDeParser expressionDeParser = new ExpressionDeParser();  
1.     StringBuffer stringBuffer = new StringBuffer();  
1.     SelectDeParser deParser = new OracleSelectDeParser(expressionDeParser, stringBuffer); //针对ORACLE的SQL生成  
1.     expressionDeParser.setSelectVisitor(deParser);  
1.     expressionDeParser.setBuffer(stringBuffer);  
1.       
1.     body.accept(deParser);  
1.     return new FilterResult(deParser.getBuffer().toString(), vc.getResultSqlParams());  
1. } catch (JSQLParserException e) {  
1.     throw new FilterException(e);  
1. }  

        CCJSqlParserManager parserManager = new CCJSqlParserManager();

        try {
            Select select = (Select) parserManager.parse(new StringReader(sql)); //解析SQL语句

            SelectBody body = select.getSelectBody();
            VisitContext vc = new VisitContext(filterContext, params);

            vc.setTableFilterFactory(tableFilterFactory);//表的字段过滤
            body.accept(new SelectVisitorImpl(vc)); //访问SQL并根据SQL中涉及的表来增强SQL

            ExpressionDeParser expressionDeParser = new ExpressionDeParser();
            StringBuffer stringBuffer = new StringBuffer();

            SelectDeParser deParser = new OracleSelectDeParser(expressionDeParser, stringBuffer); //针对ORACLE的SQL生成
            expressionDeParser.setSelectVisitor(deParser);

            expressionDeParser.setBuffer(stringBuffer);
 

            body.accept(deParser);
            return new FilterResult(deParser.getBuffer().toString(), vc.getResultSqlParams());

        } catch (JSQLParserException e) {
            throw new FilterException(e);

        }
接下去是各个VISITOR，用来访问解析后的SQL
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class SelectVisitorImpl extends AbstractVisitor implements SelectVisitor {  
1.   
1.     public SelectVisitorImpl(VisitContext ctx) {  
1.         super(ctx);  
1.     }  
1.   
1.     @SuppressWarnings("unchecked")  
1.     public void visit(PlainSelect ps) {  
1.         //SELECT ITEM访问  
1.         List<SelectItem> selectItems = ps.getSelectItems();  
1.         for (SelectItem item : selectItems) {  
1.             item.accept(new SelectItemVisitorImpl(this.getContext()));  
1.         }  
1.   
1.         //FROM访问  
1.         FromItem from = ps.getFromItem();  
1.         FromItemVisitorImpl fv = new FromItemVisitorImpl(context);  
1.         from.accept(fv);  
1.   
1.         //查询条件访问  
1.         if (ps.getWhere() != null) {  
1.             ps.getWhere().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         }  
1.   
1.         //过滤增强的条件  
1.         if (fv.getEnhancedCondition() != null) {  
1.             if (ps.getWhere() != null) {  
1.                 Expression expr = new Parenthesis(ps.getWhere());  
1.                 AndExpression and = new AndExpression(fv.getEnhancedCondition(), expr);  
1.                 ps.setWhere(and);  
1.             } else {  
1.                 ps.setWhere(fv.getEnhancedCondition());  
1.             }  
1.         }  
1.   
1.         //JOIN表的访问  
1.         List<Join> joins = ps.getJoins();  
1.         if (CollectionUtil.isNotEmpty(joins)) {  
1.             for (Join join : joins) {  
1.                 FromItemVisitorImpl tempfv = new FromItemVisitorImpl(context);  
1.                 join.getRightItem().accept(tempfv);  
1.                 if (join.getOnExpression() != null) {  
1.                     join.getOnExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.                     if (tempfv.getEnhancedCondition() != null) {  
1.                         Expression expr = new Parenthesis(join.getOnExpression());  
1.                         AndExpression and = new AndExpression(tempfv.getEnhancedCondition(), expr);  
1.                         join.setOnExpression(and);  
1.                     }  
1.                 }  
1.             }  
1.         }  
1.   
1.         //ORDER BY 访问  
1.         List<OrderByElement> elements = ps.getOrderByElements();  
1.         if (CollectionUtil.isNotEmpty(elements)) {  
1.             for (OrderByElement e : elements) {  
1.                 e.getExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.             }  
1.         }  
1.   
1.         //GROUP BY的HAVING访问  
1.         if (ps.getHaving() != null) {  
1.             ps.getHaving().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         }  
1.     }  
1.   
1.     @SuppressWarnings("unchecked")  
1.     public void visit(Union un) {  
1.         List<PlainSelect> selects = un.getPlainSelects();  
1.         for (PlainSelect select : selects) {  
1.             select.accept(new SelectVisitorImpl(this.getContext()));  
1.         }  
1.         List<OrderByElement> elements = un.getOrderByElements();  
1.         if (CollectionUtil.isNotEmpty(elements)) {  
1.             for (OrderByElement e : elements) {  
1.                 e.getExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.             }  
1.         }  
1.     }  
1.   
1. }  

public class SelectVisitorImpl extends AbstractVisitor implements SelectVisitor {

 
    public SelectVisitorImpl(VisitContext ctx) {

        super(ctx);
    }

 
    @SuppressWarnings("unchecked")

    public void visit(PlainSelect ps) {
        //SELECT ITEM访问

        List<SelectItem> selectItems = ps.getSelectItems();
        for (SelectItem item : selectItems) {

            item.accept(new SelectItemVisitorImpl(this.getContext()));
        }

 
        //FROM访问

        FromItem from = ps.getFromItem();
        FromItemVisitorImpl fv = new FromItemVisitorImpl(context);

        from.accept(fv);
 

        //查询条件访问
        if (ps.getWhere() != null) {

            ps.getWhere().accept(new ExpressionVisitorImpl(this.getContext()));
        }

 
        //过滤增强的条件

        if (fv.getEnhancedCondition() != null) {
            if (ps.getWhere() != null) {

                Expression expr = new Parenthesis(ps.getWhere());
                AndExpression and = new AndExpression(fv.getEnhancedCondition(), expr);

                ps.setWhere(and);
            } else {

                ps.setWhere(fv.getEnhancedCondition());
            }

        }
 

        //JOIN表的访问
        List<Join> joins = ps.getJoins();

        if (CollectionUtil.isNotEmpty(joins)) {
            for (Join join : joins) {

                FromItemVisitorImpl tempfv = new FromItemVisitorImpl(context);
                join.getRightItem().accept(tempfv);

                if (join.getOnExpression() != null) {
                    join.getOnExpression().accept(new ExpressionVisitorImpl(this.getContext()));

                    if (tempfv.getEnhancedCondition() != null) {
                        Expression expr = new Parenthesis(join.getOnExpression());

                        AndExpression and = new AndExpression(tempfv.getEnhancedCondition(), expr);
                        join.setOnExpression(and);

                    }
                }

            }
        }

 
        //ORDER BY 访问

        List<OrderByElement> elements = ps.getOrderByElements();
        if (CollectionUtil.isNotEmpty(elements)) {

            for (OrderByElement e : elements) {
                e.getExpression().accept(new ExpressionVisitorImpl(this.getContext()));

            }
        }

 
        //GROUP BY的HAVING访问

        if (ps.getHaving() != null) {
            ps.getHaving().accept(new ExpressionVisitorImpl(this.getContext()));

        }
    }

 
    @SuppressWarnings("unchecked")

    public void visit(Union un) {
        List<PlainSelect> selects = un.getPlainSelects();

        for (PlainSelect select : selects) {
            select.accept(new SelectVisitorImpl(this.getContext()));

        }
        List<OrderByElement> elements = un.getOrderByElements();

        if (CollectionUtil.isNotEmpty(elements)) {
            for (OrderByElement e : elements) {

                e.getExpression().accept(new ExpressionVisitorImpl(this.getContext()));
            }

        }
    }

 
}
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class SelectItemVisitorImpl extends AbstractVisitor implements SelectItemVisitor {  
1.   
1.     public SelectItemVisitorImpl(VisitContext ctx) {  
1.         super(ctx);  
1.     }  
1.   
1.     public void visit(AllColumns ac) {  
1.     }  
1.   
1.     public void visit(AllTableColumns atc) {  
1.     }  
1.   
1.     public void visit(SelectExpressionItem sei) {  
1.         sei.getExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1. }  

public class SelectItemVisitorImpl extends AbstractVisitor implements SelectItemVisitor {

 
    public SelectItemVisitorImpl(VisitContext ctx) {

        super(ctx);
    }

 
    public void visit(AllColumns ac) {

    }
 

    public void visit(AllTableColumns atc) {
    }

 
    public void visit(SelectExpressionItem sei) {

        sei.getExpression().accept(new ExpressionVisitorImpl(this.getContext()));
    }

 
}
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class ItemsListVisitorImpl extends AbstractVisitor implements ItemsListVisitor {  
1.   
1.     public ItemsListVisitorImpl(VisitContext ctx) {  
1.         super(ctx);  
1.     }  
1.   
1.     public void visit(SubSelect ss) {  
1.         ss.getSelectBody().accept(new SelectVisitorImpl(context));  
1.     }  
1.   
1.     @SuppressWarnings("unchecked")  
1.     public void visit(ExpressionList el) {  
1.         List<Expression> list = el.getExpressions();  
1.         if (CollectionUtil.isNotEmpty(list)) {  
1.             for (Expression expr : list) {  
1.                 expr.accept(new ExpressionVisitorImpl(context));  
1.             }  
1.         }  
1.     }  
1.   
1. }  

public class ItemsListVisitorImpl extends AbstractVisitor implements ItemsListVisitor {

 
    public ItemsListVisitorImpl(VisitContext ctx) {

        super(ctx);
    }

 
    public void visit(SubSelect ss) {

        ss.getSelectBody().accept(new SelectVisitorImpl(context));
    }

 
    @SuppressWarnings("unchecked")

    public void visit(ExpressionList el) {
        List<Expression> list = el.getExpressions();

        if (CollectionUtil.isNotEmpty(list)) {
            for (Expression expr : list) {

                expr.accept(new ExpressionVisitorImpl(context));
            }

        }
    }

 
}
如果FROM的内容是table的话，则根据table的增强配置对SQL增强
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class FromItemVisitorImpl extends AbstractVisitor implements FromItemVisitor {  
1.     private String varPattern = "@\\{\\s*?(\\w+)\\s*?\\}";  
1.     private Expression enhancedCondition;  
1.   
1.     public FromItemVisitorImpl(VisitContext ctx) {  
1.         super(ctx);  
1.     }  
1.   
1.     public void visit(Table table) {  
1.         Set<FieldFilter> filters = context.getTableFilterFactory().getTableFilter(table.getName());  
1.         if (filters == null) {  
1.             filters = Collections.emptySet();  
1.         }  
1.         for (FieldFilter ff : filters) {  
1.             Column c = new Column(new Table(null, table.getAlias()), ff.getFieldName());  
1.             JdbcParameter param = new JdbcParameter();  
1.             Object fieldValue = getRawValue(ff.getFieldValue(), this.context.getFilterContext());  
1.             Expression[] exps;  
1.             if ("between".equalsIgnoreCase(ff.getOperator()) || "not between".equalsIgnoreCase(ff.getOperator())) {  
1.                 Object[] objs = (Object[]) fieldValue;  
1.                 this.getContext().getResultSqlParams().add(objs[0]);  
1.                 this.getContext().getResultSqlParams().add(objs[1]);  
1.                 exps = new Expression[] { c, param, param };  
1.             } else if ("is null".equalsIgnoreCase(ff.getOperator()) || "is not null".equalsIgnoreCase(ff.getOperator())) {  
1.                 exps = new Expression[] { c };  
1.             } else {  
1.                 this.getContext().getResultSqlParams().add(fieldValue);  
1.                 exps = new Expression[] { c, param };  
1.             }  
1.             Expression operator = this.getOperator(ff.getOperator(), exps);  
1.             if (this.enhancedCondition != null) {  
1.                 enhancedCondition = new AndExpression(enhancedCondition, operator);  
1.             } else {  
1.                 enhancedCondition = operator;  
1.             }  
1.         }  
1.     }  
1.   
1.     public void visit(SubSelect ss) {  
1.         ss.getSelectBody().accept(new SelectVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     public void visit(SubJoin sj) {  
1.         Join join = sj.getJoin();  
1.         join.getRightItem().accept(new FromItemVisitorImpl(this.getContext()));  
1.         join.getOnExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     private Expression getOperator(String op, Expression[] exp) {  
1.         if ("=".equals(op)) {  
1.             EqualsTo eq = new EqualsTo();  
1.             eq.setLeftExpression(exp[0]);  
1.             eq.setRightExpression(exp[1]);  
1.             return eq;  
1.         } else if (">".equals(op)) {  
1.             GreaterThan gt = new GreaterThan();  
1.             gt.setLeftExpression(exp[0]);  
1.             gt.setRightExpression(exp[1]);  
1.             return gt;  
1.         } else if (">=".equals(op)) {  
1.             GreaterThanEquals geq = new GreaterThanEquals();  
1.             geq.setLeftExpression(exp[0]);  
1.             geq.setRightExpression(exp[1]);  
1.             return geq;  
1.         } else if ("<".equals(op)) {  
1.             MinorThan mt = new MinorThan();  
1.             mt.setLeftExpression(exp[0]);  
1.             mt.setRightExpression(exp[1]);  
1.             return mt;  
1.         } else if ("<=".equals(op)) {  
1.             MinorThanEquals leq = new MinorThanEquals();  
1.             leq.setLeftExpression(exp[0]);  
1.             leq.setRightExpression(exp[1]);  
1.             return leq;  
1.         } else if ("<>".equals(op)) {  
1.             NotEqualsTo neq = new NotEqualsTo();  
1.             neq.setLeftExpression(exp[0]);  
1.             neq.setRightExpression(exp[1]);  
1.             return neq;  
1.         } else if ("is null".equalsIgnoreCase(op)) {  
1.             IsNullExpression isNull = new IsNullExpression();  
1.             isNull.setNot(false);  
1.             isNull.setLeftExpression(exp[0]);  
1.             return isNull;  
1.         } else if ("is not null".equalsIgnoreCase(op)) {  
1.             IsNullExpression isNull = new IsNullExpression();  
1.             isNull.setNot(true);  
1.             isNull.setLeftExpression(exp[0]);  
1.             return isNull;  
1.         } else if ("like".equalsIgnoreCase(op)) {  
1.             LikeExpression like = new LikeExpression();  
1.             like.setNot(false);  
1.             like.setLeftExpression(exp[0]);  
1.             like.setRightExpression(exp[1]);  
1.             return like;  
1.         } else if ("not like".equalsIgnoreCase(op)) {  
1.             LikeExpression nlike = new LikeExpression();  
1.             nlike.setNot(true);  
1.             nlike.setLeftExpression(exp[0]);  
1.             nlike.setRightExpression(exp[1]);  
1.             return nlike;  
1.         } else if ("between".equalsIgnoreCase(op)) {  
1.             Between bt = new Between();  
1.             bt.setNot(false);  
1.             bt.setLeftExpression(exp[0]);  
1.             bt.setBetweenExpressionStart(exp[1]);  
1.             bt.setBetweenExpressionEnd(exp[2]);  
1.             return bt;  
1.         } else if ("not between".equalsIgnoreCase(op)) {  
1.             Between bt = new Between();  
1.             bt.setNot(true);  
1.             bt.setLeftExpression(exp[0]);  
1.             bt.setBetweenExpressionStart(exp[1]);  
1.             bt.setBetweenExpressionEnd(exp[2]);  
1.             return bt;  
1.         }  
1.         throw new FilterException("Unknown operator:" + op);  
1.     }  
1.   
1.     protected Object getRawValue(Object value, Map<String, Object> context) {  
1.         if (context == null) {  
1.             return value;  
1.         }  
1.         if (value instanceof String) {  
1.             String v = (String) value;  
1.             Pattern pattern = Pattern.compile(varPattern);  
1.             Matcher matcher = pattern.matcher(v);  
1.             if (matcher.find()) {  
1.                 return context.get(matcher.group(1));  
1.             }  
1.         }  
1.         return value;  
1.     }  
1.   
1.     public Expression getEnhancedCondition() {  
1.         return enhancedCondition;  
1.     }  
1.   
1. }  

public class FromItemVisitorImpl extends AbstractVisitor implements FromItemVisitor {

    private String varPattern = "@\\{\\s*?(\\w+)\\s*?\\}";
    private Expression enhancedCondition;

 
    public FromItemVisitorImpl(VisitContext ctx) {

        super(ctx);
    }

 
    public void visit(Table table) {

        Set<FieldFilter> filters = context.getTableFilterFactory().getTableFilter(table.getName());
        if (filters == null) {

            filters = Collections.emptySet();
        }

        for (FieldFilter ff : filters) {
            Column c = new Column(new Table(null, table.getAlias()), ff.getFieldName());

            JdbcParameter param = new JdbcParameter();
            Object fieldValue = getRawValue(ff.getFieldValue(), this.context.getFilterContext());

            Expression[] exps;
            if ("between".equalsIgnoreCase(ff.getOperator()) || "not between".equalsIgnoreCase(ff.getOperator())) {

                Object[] objs = (Object[]) fieldValue;
                this.getContext().getResultSqlParams().add(objs[0]);

                this.getContext().getResultSqlParams().add(objs[1]);
                exps = new Expression[] { c, param, param };

            } else if ("is null".equalsIgnoreCase(ff.getOperator()) || "is not null".equalsIgnoreCase(ff.getOperator())) {
                exps = new Expression[] { c };

            } else {
                this.getContext().getResultSqlParams().add(fieldValue);

                exps = new Expression[] { c, param };
            }

            Expression operator = this.getOperator(ff.getOperator(), exps);
            if (this.enhancedCondition != null) {

                enhancedCondition = new AndExpression(enhancedCondition, operator);
            } else {

                enhancedCondition = operator;
            }

        }
    }

 
    public void visit(SubSelect ss) {

        ss.getSelectBody().accept(new SelectVisitorImpl(this.getContext()));
    }

 
    public void visit(SubJoin sj) {

        Join join = sj.getJoin();
        join.getRightItem().accept(new FromItemVisitorImpl(this.getContext()));

        join.getOnExpression().accept(new ExpressionVisitorImpl(this.getContext()));
    }

 
    private Expression getOperator(String op, Expression[] exp) {

        if ("=".equals(op)) {
            EqualsTo eq = new EqualsTo();

            eq.setLeftExpression(exp[0]);
            eq.setRightExpression(exp[1]);

            return eq;
        } else if (">".equals(op)) {

            GreaterThan gt = new GreaterThan();
            gt.setLeftExpression(exp[0]);

            gt.setRightExpression(exp[1]);
            return gt;

        } else if (">=".equals(op)) {
            GreaterThanEquals geq = new GreaterThanEquals();

            geq.setLeftExpression(exp[0]);
            geq.setRightExpression(exp[1]);

            return geq;
        } else if ("<".equals(op)) {

            MinorThan mt = new MinorThan();
            mt.setLeftExpression(exp[0]);

            mt.setRightExpression(exp[1]);
            return mt;

        } else if ("<=".equals(op)) {
            MinorThanEquals leq = new MinorThanEquals();

            leq.setLeftExpression(exp[0]);
            leq.setRightExpression(exp[1]);

            return leq;
        } else if ("<>".equals(op)) {

            NotEqualsTo neq = new NotEqualsTo();
            neq.setLeftExpression(exp[0]);

            neq.setRightExpression(exp[1]);
            return neq;

        } else if ("is null".equalsIgnoreCase(op)) {
            IsNullExpression isNull = new IsNullExpression();

            isNull.setNot(false);
            isNull.setLeftExpression(exp[0]);

            return isNull;
        } else if ("is not null".equalsIgnoreCase(op)) {

            IsNullExpression isNull = new IsNullExpression();
            isNull.setNot(true);

            isNull.setLeftExpression(exp[0]);
            return isNull;

        } else if ("like".equalsIgnoreCase(op)) {
            LikeExpression like = new LikeExpression();

            like.setNot(false);
            like.setLeftExpression(exp[0]);

            like.setRightExpression(exp[1]);
            return like;

        } else if ("not like".equalsIgnoreCase(op)) {
            LikeExpression nlike = new LikeExpression();

            nlike.setNot(true);
            nlike.setLeftExpression(exp[0]);

            nlike.setRightExpression(exp[1]);
            return nlike;

        } else if ("between".equalsIgnoreCase(op)) {
            Between bt = new Between();

            bt.setNot(false);
            bt.setLeftExpression(exp[0]);

            bt.setBetweenExpressionStart(exp[1]);
            bt.setBetweenExpressionEnd(exp[2]);

            return bt;
        } else if ("not between".equalsIgnoreCase(op)) {

            Between bt = new Between();
            bt.setNot(true);

            bt.setLeftExpression(exp[0]);
            bt.setBetweenExpressionStart(exp[1]);

            bt.setBetweenExpressionEnd(exp[2]);
            return bt;

        }
        throw new FilterException("Unknown operator:" + op);

    }
 

    protected Object getRawValue(Object value, Map<String, Object> context) {
        if (context == null) {

            return value;
        }

        if (value instanceof String) {
            String v = (String) value;

            Pattern pattern = Pattern.compile(varPattern);
            Matcher matcher = pattern.matcher(v);

            if (matcher.find()) {
                return context.get(matcher.group(1));

            }
        }

        return value;
    }

 
    public Expression getEnhancedCondition() {

        return enhancedCondition;
    }

 
}
1）对JDBC parameter做了处理，如果参数为NULL则自动忽略该parameter，忽略后需要处理and or between 等情况
   如：where name=? and age=? ，假如name对应的参数为null，则条件改为where 1=1 and age=?，如果是or的话则改为 where 1=0 or age=?
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class ExpressionVisitorImpl extends AbstractVisitor implements ExpressionVisitor {  
1.   
1.     public ExpressionVisitorImpl(VisitContext ctx) {  
1.         super(ctx);  
1.     }  
1.   
1.     public void visit(NullValue nv) {  
1.     }  
1.   
1.     public void visit(Function f) {  
1.     }  
1.   
1.     public void visit(InverseExpression ie) {  
1.     }  
1.   
1.     public void visit(JdbcParameter jp) {  
1.         this.getContext().getResultSqlParams().add(context.removeFirstParam());  
1.     }  
1.   
1.     public void visit(DoubleValue dv) {  
1.     }  
1.   
1.     public void visit(LongValue lv) {  
1.     }  
1.   
1.     public void visit(DateValue dv) {  
1.     }  
1.   
1.     public void visit(TimeValue tv) {  
1.     }  
1.   
1.     public void visit(TimestampValue tv) {  
1.     }  
1.   
1.     public void visit(Parenthesis parenthesis) {  
1.         ExpressionVisitorImpl ev = new ExpressionVisitorImpl(context);  
1.         parenthesis.getExpression().accept(ev);  
1.         if (ev.isNotValid()) {  
1.             parenthesis.setExpression(this.createTrueEquals());  
1.         }  
1.     }  
1.   
1.     public void visit(StringValue s) {  
1.     }  
1.   
1.     public void visit(Addition a) {  
1.     }  
1.   
1.     public void visit(Division d) {  
1.     }  
1.   
1.     public void visit(Multiplication m) {  
1.     }  
1.   
1.     public void visit(Subtraction s) {  
1.     }  
1.   
1.     public void visit(AndExpression and) {  
1.         ExpressionVisitorImpl left = new ExpressionVisitorImpl(this.getContext());  
1.         and.getLeftExpression().accept(left);  
1.         if (left.isNotValid()) {  
1.             and.setLeftExpression(this.createTrueEquals());  
1.         }  
1.         ExpressionVisitorImpl right = new ExpressionVisitorImpl(this.getContext());  
1.         and.getRightExpression().accept(right);  
1.         if (right.isNotValid()) {  
1.             and.setRightExpression(this.createTrueEquals());  
1.         }  
1.     }  
1.   
1.     public void visit(OrExpression or) {  
1.         ExpressionVisitorImpl left = new ExpressionVisitorImpl(this.getContext());  
1.         or.getLeftExpression().accept(left);  
1.         if (left.isNotValid()) {  
1.             or.setLeftExpression(this.createFalseEquals());  
1.         }  
1.         ExpressionVisitorImpl right = new ExpressionVisitorImpl(this.getContext());  
1.         or.getRightExpression().accept(right);  
1.         if (right.isNotValid()) {  
1.             or.setRightExpression(this.createFalseEquals());  
1.         }  
1.     }  
1.   
1.     public void visit(Between btw) {  
1.         Expression start = btw.getBetweenExpressionStart();  
1.         Expression end = btw.getBetweenExpressionEnd();  
1.         if (start instanceof JdbcParameter && end instanceof JdbcParameter) {  
1.             Object o1 = this.context.getFirstParam();  
1.             Object o2 = this.context.getParam(1);  
1.             if (o1 == null || o2 == null) {  
1.                 this.context.removeFirstParam();  
1.                 this.context.removeFirstParam();  
1.                 this.setValid(false);  
1.                 return;  
1.             }  
1.         } else if (start instanceof JdbcParameter || end instanceof JdbcParameter) {  
1.             Object o1 = this.context.getFirstParam();  
1.             if (o1 == null) {  
1.                 this.context.removeFirstParam();  
1.                 this.setValid(false);  
1.                 return;  
1.             }  
1.         }  
1.         btw.getLeftExpression().accept(new ExpressionVisitorImpl(context));  
1.         btw.getBetweenExpressionStart().accept(new ExpressionVisitorImpl(context));  
1.         btw.getBetweenExpressionEnd().accept(new ExpressionVisitorImpl(context));  
1.     }  
1.   
1.     public void visit(EqualsTo eq) {  
1.         if (eq.getRightExpression() instanceof JdbcParameter) {  
1.             Object o = this.context.getFirstParam();  
1.             if (o == null) {  
1.                 this.setValid(false);  
1.                 this.getContext().removeFirstParam();  
1.                 return;  
1.             }  
1.         }  
1.         eq.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         eq.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     public void visit(GreaterThan gt) {  
1.         if (gt.getRightExpression() instanceof JdbcParameter) {  
1.             Object o = this.context.getFirstParam();  
1.             if (o == null) {  
1.                 this.setValid(false);  
1.                 this.getContext().removeFirstParam();  
1.             }  
1.         }  
1.         gt.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         gt.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     public void visit(GreaterThanEquals gte) {  
1.         if (gte.getRightExpression() instanceof JdbcParameter) {  
1.             Object o = this.context.getFirstParam();  
1.             if (o == null) {  
1.                 this.setValid(false);  
1.                 this.getContext().removeFirstParam();  
1.             }  
1.         }  
1.         gte.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         gte.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     public void visit(InExpression in) {  
1.         ItemsList list = in.getItemsList();  
1.         list.accept(new ItemsListVisitorImpl(context));  
1.     }  
1.   
1.     public void visit(IsNullExpression ine) {  
1.     }  
1.   
1.     public void visit(LikeExpression le) {  
1.         if (le.getRightExpression() instanceof JdbcParameter) {  
1.             Object o = this.context.getFirstParam();  
1.             if (o == null) {  
1.                 this.setValid(false);  
1.                 this.getContext().removeFirstParam();  
1.             }  
1.         }  
1.         le.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         le.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     public void visit(MinorThan mt) {  
1.         if (mt.getRightExpression() instanceof JdbcParameter) {  
1.             Object o = this.context.getFirstParam();  
1.             if (o == null) {  
1.                 this.setValid(false);  
1.                 this.getContext().removeFirstParam();  
1.             }  
1.         }  
1.         mt.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         mt.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     public void visit(MinorThanEquals mte) {  
1.         if (mte.getRightExpression() instanceof JdbcParameter) {  
1.             Object o = this.context.getFirstParam();  
1.             if (o == null) {  
1.                 this.setValid(false);  
1.                 this.getContext().removeFirstParam();  
1.             }  
1.         }  
1.         mte.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         mte.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     public void visit(NotEqualsTo neq) {  
1.         if (neq.getRightExpression() instanceof JdbcParameter) {  
1.             Object o = this.context.getFirstParam();  
1.             if (o == null) {  
1.                 this.setValid(false);  
1.                 this.getContext().removeFirstParam();  
1.                 return;  
1.             }  
1.         }  
1.         neq.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.         neq.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));  
1.     }  
1.   
1.     public void visit(Column c) {  
1.     }  
1.   
1.     public void visit(SubSelect ss) {  
1.         ss.getSelectBody().accept(new SelectVisitorImpl(context));  
1.     }  
1.   
1.     public void visit(CaseExpression ce) {  
1.     }  
1.   
1.     public void visit(WhenClause wc) {  
1.     }  
1.   
1.     public void visit(ExistsExpression ee) {  
1.     }  
1.   
1.     public void visit(AllComparisonExpression ace) {  
1.     }  
1.   
1.     public void visit(AnyComparisonExpression ace) {  
1.     }  
1.   
1.     public void visit(Concat c) {  
1.     }  
1.   
1.     public void visit(Matches m) {  
1.     }  
1.   
1.     public void visit(BitwiseAnd ba) {  
1.     }  
1.   
1.     public void visit(BitwiseOr bo) {  
1.     }  
1.   
1.     public void visit(BitwiseXor bx) {  
1.     }  
1.   
1.     private EqualsTo createTrueEquals() {  
1.         EqualsTo eq = new EqualsTo();  
1.         eq.setLeftExpression(new LongValue("1"));  
1.         eq.setRightExpression(new LongValue("1"));  
1.         return eq;  
1.     }  
1.   
1.     private EqualsTo createFalseEquals() {  
1.         EqualsTo eq = new EqualsTo();  
1.         eq.setLeftExpression(new LongValue("1"));  
1.         eq.setRightExpression(new LongValue("0"));  
1.         return eq;  
1.     }  
1.   
1. }  

public class ExpressionVisitorImpl extends AbstractVisitor implements ExpressionVisitor {

 
    public ExpressionVisitorImpl(VisitContext ctx) {

        super(ctx);
    }

 
    public void visit(NullValue nv) {

    }
 

    public void visit(Function f) {
    }

 
    public void visit(InverseExpression ie) {

    }
 

    public void visit(JdbcParameter jp) {
        this.getContext().getResultSqlParams().add(context.removeFirstParam());

    }
 

    public void visit(DoubleValue dv) {
    }

 
    public void visit(LongValue lv) {

    }
 

    public void visit(DateValue dv) {
    }

 
    public void visit(TimeValue tv) {

    }
 

    public void visit(TimestampValue tv) {
    }

 
    public void visit(Parenthesis parenthesis) {

        ExpressionVisitorImpl ev = new ExpressionVisitorImpl(context);
        parenthesis.getExpression().accept(ev);

        if (ev.isNotValid()) {
            parenthesis.setExpression(this.createTrueEquals());

        }
    }

 
    public void visit(StringValue s) {

    }
 

    public void visit(Addition a) {
    }

 
    public void visit(Division d) {

    }
 

    public void visit(Multiplication m) {
    }

 
    public void visit(Subtraction s) {

    }
 

    public void visit(AndExpression and) {
        ExpressionVisitorImpl left = new ExpressionVisitorImpl(this.getContext());

        and.getLeftExpression().accept(left);
        if (left.isNotValid()) {

            and.setLeftExpression(this.createTrueEquals());
        }

        ExpressionVisitorImpl right = new ExpressionVisitorImpl(this.getContext());
        and.getRightExpression().accept(right);

        if (right.isNotValid()) {
            and.setRightExpression(this.createTrueEquals());

        }
    }

 
    public void visit(OrExpression or) {

        ExpressionVisitorImpl left = new ExpressionVisitorImpl(this.getContext());
        or.getLeftExpression().accept(left);

        if (left.isNotValid()) {
            or.setLeftExpression(this.createFalseEquals());

        }
        ExpressionVisitorImpl right = new ExpressionVisitorImpl(this.getContext());

        or.getRightExpression().accept(right);
        if (right.isNotValid()) {

            or.setRightExpression(this.createFalseEquals());
        }

    }
 

    public void visit(Between btw) {
        Expression start = btw.getBetweenExpressionStart();

        Expression end = btw.getBetweenExpressionEnd();
        if (start instanceof JdbcParameter && end instanceof JdbcParameter) {

            Object o1 = this.context.getFirstParam();
            Object o2 = this.context.getParam(1);

            if (o1 == null || o2 == null) {
                this.context.removeFirstParam();

                this.context.removeFirstParam();
                this.setValid(false);

                return;
            }

        } else if (start instanceof JdbcParameter || end instanceof JdbcParameter) {
            Object o1 = this.context.getFirstParam();

            if (o1 == null) {
                this.context.removeFirstParam();

                this.setValid(false);
                return;

            }
        }

        btw.getLeftExpression().accept(new ExpressionVisitorImpl(context));
        btw.getBetweenExpressionStart().accept(new ExpressionVisitorImpl(context));

        btw.getBetweenExpressionEnd().accept(new ExpressionVisitorImpl(context));
    }

 
    public void visit(EqualsTo eq) {

        if (eq.getRightExpression() instanceof JdbcParameter) {
            Object o = this.context.getFirstParam();

            if (o == null) {
                this.setValid(false);

                this.getContext().removeFirstParam();
                return;

            }
        }

        eq.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));
        eq.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));

    }
 

    public void visit(GreaterThan gt) {
        if (gt.getRightExpression() instanceof JdbcParameter) {

            Object o = this.context.getFirstParam();
            if (o == null) {

                this.setValid(false);
                this.getContext().removeFirstParam();

            }
        }

        gt.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));
        gt.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));

    }
 

    public void visit(GreaterThanEquals gte) {
        if (gte.getRightExpression() instanceof JdbcParameter) {

            Object o = this.context.getFirstParam();
            if (o == null) {

                this.setValid(false);
                this.getContext().removeFirstParam();

            }
        }

        gte.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));
        gte.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));

    }
 

    public void visit(InExpression in) {
        ItemsList list = in.getItemsList();

        list.accept(new ItemsListVisitorImpl(context));
    }

 
    public void visit(IsNullExpression ine) {

    }
 

    public void visit(LikeExpression le) {
        if (le.getRightExpression() instanceof JdbcParameter) {

            Object o = this.context.getFirstParam();
            if (o == null) {

                this.setValid(false);
                this.getContext().removeFirstParam();

            }
        }

        le.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));
        le.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));

    }
 

    public void visit(MinorThan mt) {
        if (mt.getRightExpression() instanceof JdbcParameter) {

            Object o = this.context.getFirstParam();
            if (o == null) {

                this.setValid(false);
                this.getContext().removeFirstParam();

            }
        }

        mt.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));
        mt.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));

    }
 

    public void visit(MinorThanEquals mte) {
        if (mte.getRightExpression() instanceof JdbcParameter) {

            Object o = this.context.getFirstParam();
            if (o == null) {

                this.setValid(false);
                this.getContext().removeFirstParam();

            }
        }

        mte.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));
        mte.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));

    }
 

    public void visit(NotEqualsTo neq) {
        if (neq.getRightExpression() instanceof JdbcParameter) {

            Object o = this.context.getFirstParam();
            if (o == null) {

                this.setValid(false);
                this.getContext().removeFirstParam();

                return;
            }

        }
        neq.getLeftExpression().accept(new ExpressionVisitorImpl(this.getContext()));

        neq.getRightExpression().accept(new ExpressionVisitorImpl(this.getContext()));
    }

 
    public void visit(Column c) {

    }
 

    public void visit(SubSelect ss) {
        ss.getSelectBody().accept(new SelectVisitorImpl(context));

    }
 

    public void visit(CaseExpression ce) {
    }

 
    public void visit(WhenClause wc) {

    }
 

    public void visit(ExistsExpression ee) {
    }

 
    public void visit(AllComparisonExpression ace) {

    }
 

    public void visit(AnyComparisonExpression ace) {
    }

 
    public void visit(Concat c) {

    }
 

    public void visit(Matches m) {
    }

 
    public void visit(BitwiseAnd ba) {

    }
 

    public void visit(BitwiseOr bo) {
    }

 
    public void visit(BitwiseXor bx) {

    }
 

    private EqualsTo createTrueEquals() {
        EqualsTo eq = new EqualsTo();

        eq.setLeftExpression(new LongValue("1"));
        eq.setRightExpression(new LongValue("1"));

        return eq;
    }

 
    private EqualsTo createFalseEquals() {

        EqualsTo eq = new EqualsTo();
        eq.setLeftExpression(new LongValue("1"));

        eq.setRightExpression(new LongValue("0"));
        return eq;

    }
 

}
增强后SQL语句的重新生成，根据ORACLE的语法重写了几个生成的方法
Java代码  [![收藏代码]()![]()]( "收藏这段代码")

1. public class OracleSelectDeParser extends SelectDeParser {  
1.   
1.     public OracleSelectDeParser(ExpressionDeParser expressionDeParser, StringBuffer sb) {  
1.         super(expressionDeParser, sb);  
1.     }  
1.   
1.     /** 
1.      * 重写父类方法，去掉父类方法中table前的as 
1.      */  
1.     public void visit(Table tableName) {  
1.         buffer.append(tableName.getWholeTableName());  
1.         String alias = tableName.getAlias();  
1.         if (alias != null && StringUtil.isNotEmpty(alias)) {  
1.             buffer.append(" ");  
1.             buffer.append(alias);  
1.         }  
1.     }  
1.   
1.     /** 
1.      * 重写父类方法，在JOIN之前增加空格 
1.      */  
1.     @SuppressWarnings("unchecked")  
1.     public void deparseJoin(Join join) {  
1.         if (join.isSimple()) {  
1.             buffer.append(", ");  
1.         } else {  
1.             buffer.append(" ");  
1.             if (join.isRight()) {  
1.                 buffer.append("RIGHT ");  
1.             } else if (join.isNatural()) {  
1.                 buffer.append("NATURAL ");  
1.             } else if (join.isFull()) {  
1.                 buffer.append("FULL ");  
1.             } else if (join.isLeft()) {  
1.                 buffer.append("LEFT ");  
1.             }  
1.             if (join.isOuter()) {  
1.                 buffer.append("OUTER ");  
1.             } else if (join.isInner()) {  
1.                 buffer.append("INNER ");  
1.             }  
1.             buffer.append("JOIN ");  
1.         }  
1.   
1.         FromItem fromItem = join.getRightItem();  
1.         fromItem.accept(this);  
1.         if (join.getOnExpression() != null) {  
1.             buffer.append(" ON ");  
1.             join.getOnExpression().accept(expressionVisitor);  
1.         }  
1.         if (join.getUsingColumns() != null) {  
1.             buffer.append(" USING ( ");  
1.             for (Iterator<Column> iterator = join.getUsingColumns().iterator(); iterator.hasNext();) {  
1.                 Column column = iterator.next();  
1.                 buffer.append(column.getWholeColumnName());  
1.                 if (iterator.hasNext()) {  
1.                     buffer.append(" ,");  
1.                 }  
1.             }  
1.             buffer.append(")");  
1.         }  
1.     }  
1.   
1. }  

public class OracleSelectDeParser extends SelectDeParser {

 
    public OracleSelectDeParser(ExpressionDeParser expressionDeParser, StringBuffer sb) {

        super(expressionDeParser, sb);
    }

 
    /**

     * 重写父类方法，去掉父类方法中table前的as
     */

    public void visit(Table tableName) {
        buffer.append(tableName.getWholeTableName());

        String alias = tableName.getAlias();
        if (alias != null && StringUtil.isNotEmpty(alias)) {

            buffer.append(" ");
            buffer.append(alias);

        }
    }

 
    /**

     * 重写父类方法，在JOIN之前增加空格
     */

    @SuppressWarnings("unchecked")
    public void deparseJoin(Join join) {

        if (join.isSimple()) {
            buffer.append(", ");

        } else {
            buffer.append(" ");

            if (join.isRight()) {
                buffer.append("RIGHT ");

            } else if (join.isNatural()) {
                buffer.append("NATURAL ");

            } else if (join.isFull()) {
                buffer.append("FULL ");

            } else if (join.isLeft()) {
                buffer.append("LEFT ");

            }
            if (join.isOuter()) {

                buffer.append("OUTER ");
            } else if (join.isInner()) {

                buffer.append("INNER ");
            }

            buffer.append("JOIN ");
        }

 
        FromItem fromItem = join.getRightItem();

        fromItem.accept(this);
        if (join.getOnExpression() != null) {

            buffer.append(" ON ");
            join.getOnExpression().accept(expressionVisitor);

        }
        if (join.getUsingColumns() != null) {

            buffer.append(" USING ( ");
            for (Iterator<Column> iterator = join.getUsingColumns().iterator(); iterator.hasNext();) {

                Column column = iterator.next();
                buffer.append(column.getWholeColumnName());

                if (iterator.hasNext()) {
                    buffer.append(" ,");

                }
            }

            buffer.append(")");
        }

    }
 

}
