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
```
function _new(fn,...args){
    const obj=Object.create(fn.prototype)
    const ret=fn.apply(obj,arg)
    return res instanceof Object?ret:obj
}
```
### promise ###
```
class Prmoise{
    constructor(executor){
        this.state='pending'
        this.value=undefined
        this.reason=undefined
        this.onRejectedCallbacks=[]
        this.onResolvedCallbacks=[]
        let resolve=value=>{
            if(this.state==='pending'){
                this.state='fulfilled
                this.value=value
                this.onResolvedCallbacks.forEach(fn=>fn())
            }
        }
        let reject=reason=>{
            if(this.state==='pending'){
                this.state='rejected'
                this.reason=reason
                this.onRejectedCallbacks.forEach(fn=>fn())
            }
        }
        try{
            executor(resolve,reject)
        }catch(err){
            reject(err)
        }
    }
    then(onFulfilled,onRejected){
        onFulfilled=typeof onFulfilled==='function'?onFulfilled:value=>value
        onRejected=typeof onRejected==='function'?onRejected:err=>{throw err}
        let promise2=new Promise(resolve,reject=>{
            if(this.state==='fulfilled'){
                setTimeout(()=>{
                    try{
                        let x=onFulfilled(this.value)
                        resolvePromise(promise2,x,resolve,reject)

                    }catch(e){
                        reject(e)
                    }
                },0)
            }
            if(this.state==='rejected'){
                setTimeout(()=>{
                    try{

                        let x=onReject(this.reason)
                        resolvePromise(promise2,x,resolve,reject)
                    }catch(e){
                        reject(e)
                    }
                },0)
            }
            if(this.state==='pending'){
                this.onResolveCallbacks.push(()=>{
                setTimeout(()=>{
                    try{
                    let x=onFulfilled(this.value)
                    resolvePromise(promise2,x,resolve,reject)

                    }catch(e){
                        reject(e)
                    }
                },0)
                })
                this.onRejectedCallbacks.push(()=>{
                    setTimeout(()=>{
                    try{
                        let x=onRejected(this.value)
                        resolvePromise(promise2,x,resolve,reject)

                    }catch(e){
                        reject(e)
                    }
                },0)
                })
            }
        })
        return promise2
    }
}
function resolvePromise(promise2,x,resolve,reject){
    if(x===promise){
        //循环引用报错
        return reject(nre TypeError('chaining cycle detected for promise'))
    }
    //防止多次调用
    let called
    if(x!=null && typeof x==='object' || typeof x==='function'){
        //x不是null且x是对象或函数
        try{
            let then=x.then
            if(typeof then==='function'){
                then.call(x,y=>{
                    if(called) return
                    called=true
                    resolvePromise(promise2,y,resolve,reject)
                },err=>{
                    if(called) return 
                    called=true
                    reject(err)
                })
            }else{
                resolve(x)
            }
        }catch(e){
            if(called) return 
            called=true
            reject(e)
        }
    }else{
        resolve(x)
    }
}
```
