Promise并发控制主要是希望一次不要执行太多Promise，所以传进去的参数不能是Promise，因为Promise是同步的，所以要把Promise的创建包装到一个函数里面，把Promise的创建交给控制器处理。
+ 只要没有达到最大限制，就应该把未执行的Promise取出来执行，直到并行执行的Promise达到了最大限制
+ 整个Promise返回的结果应该和Promise.all的结果是一样的
+ 一旦有一个Promise异常，则整个Promise应该reject
<pre><code>PromiseAll = function (promises, limit) {
    return new Promise((resolve, reject) => {
        const length = promises.length
        if (length === limit) return Promise.all(promises.map(item => item()))
        let curIndex = -1
        let activeCount = 0
        let doneIndex = 0
        const getNextPromise = () => promises[++curIndex]
        const result = []
        function doneCallback () {
            if (activeCount < limit) {
                while (activeCount < limit) {
                    const next = getNextPromise()
                    let index = curIndex
                    activeCount++
                    next().then(res => {
                        doneIndex++
                        result[index] = res
                        activeCount--
                        if (curIndex < length - 1) doneCallback()
                        if (doneIndex === length) {
                            resolve(result)
                        }
                    }).catch((err) => {
                        reject(err)
                    })
                }
            }
        }
        doneCallback()
    })
}</code></pre>
下面我通过手动创建20个Promise对象包装器，包装器执行后会返回一个Promise
<pre><code>const promiseStore = []
for (let i = 0; i < 20; i++) {
    promiseStore.push(() => {
        return new Promise((resolve, reject) => {
            console.log(i, 'promise 任务开始了')
            setTimeout(() => {
                console.log(i)
                console.log(new Date().getTime())
                resolve(i)
            }, 100 + Math.floor(Math.random() * 2000))
        })
    })
}
const rep = PromiseAll(promiseStore, 8).then(res => console.log(res))</code></pre>
在浏览器控制条可以看到Promise是依次执行的，但是执行结束的顺序是不确定的，而最终整个Promise的返回结果顺序是对的
