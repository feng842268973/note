### bind ###
```
Function.prototype.myBind=function(context){
    if(typeof this!='function'){
        throw new TypeError('error')
    }
    const _this=this
    const args=[...arguments].slice(1)
    return function F(){
        if(this instanceof F){
           return new _this(...args,...arguments) 
        }
        return _this.apply(context,args.concat(...arguments))
    }
}
```
### call ###
```
Function.prototype.myCall=function(context){
    if(typeof this!='function'){
        throw new TypeError('error')
    }
    context=context||window
    context.fn=this
    const args=[...arguments].slice(1)
    const result=context.fn(...args)
    delete context.fn
    return result
}
```
### apply ###
```
Function.prototype.myCall=function(context){
    if(typeof this!='function'){
        throw new TypeError('error')
    }
    context=context||window
    context.fn=this
    let result
    if(arguments[1]){
        result=context.fn(...arguments[1])
    }else{
        result=context.fn()
    }
    delete context.fn
    return result
}
```
### new ###
function _new(fn,...args){
    const obj=Object.create(fn.prototype)
    const ret=fn.apply(obj,arg)
    return res instanceof Object?ret:obj
}

