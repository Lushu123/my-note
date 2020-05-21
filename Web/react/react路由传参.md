### 1、通过state传参（但在新页面刷新会丢失数据）

```javascript
this.props.history.push({
            pathname: `/adminMain/test/updateTestPaper/`,
    	    state
        })
读取参数用:this.props.location.state
```

### 3、通过query传参

```javascript
<Route path='/query' component={Query}/>
<Link to={{ path : ' /query' , query : { name : 'sunny' }}}>
    
this.props.history.push({pathname:"/query",query: { name : 'sunny' }});
读取参数用: this.props.location.query.name
```



### 2、通过params传参

```javascript
<Route path='/path/:name' component={Path}/>
<link to="/path/2">xxx</Link>

this.props.history.push({pathname:"/path/" + name});
读取参数用:this.props.match.params.name
```

### 