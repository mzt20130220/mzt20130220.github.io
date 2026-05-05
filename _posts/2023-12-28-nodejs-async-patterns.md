---
title: "Node.js 异步编程模式"
date: 2023-12-28
categories: [后端开发]
tags: [Node.js, 异步编程]
excerpt: "从回调地狱到 Promise，再到 async/await，Node.js 的异步编程方式经历了多次演进。本文梳理了各种异步模式的优缺点，帮助你选择最适合项目的方案。"
---

## 回调函数

回调函数是 Node.js 最早的异步编程模式。虽然简单，但容易导致回调地狱问题：

```javascript
fs.readFile('file1.txt', 'utf8', function(err, data1) {
    if (err) throw err;
    fs.readFile('file2.txt', 'utf8', function(err, data2) {
        if (err) throw err;
        fs.readFile('file3.txt', 'utf8', function(err, data3) {
            if (err) throw err;
            console.log(data1 + data2 + data3);
        });
    });
});
```

## Promise

Promise 是 ES6 引入的异步编程解决方案。它可以链式调用，解决了回调地狱问题：

```javascript
fs.promises.readFile('file1.txt', 'utf8')
    .then(data1 => {
        return fs.promises.readFile('file2.txt', 'utf8');
    })
    .then(data2 => {
        return fs.promises.readFile('file3.txt', 'utf8');
    })
    .then(data3 => {
        console.log(data1 + data2 + data3);
    })
    .catch(err => {
        console.error('Error:', err);
    });
```

## async/await

async/await 是 ES8 引入的语法糖，它让异步代码看起来像同步代码，大大提高了代码的可读性：

```javascript
async function readFiles() {
    try {
        const data1 = await fs.promises.readFile('file1.txt', 'utf8');
        const data2 = await fs.promises.readFile('file2.txt', 'utf8');
        const data3 = await fs.promises.readFile('file3.txt', 'utf8');
        console.log(data1 + data2 + data3);
    } catch (err) {
        console.error('Error:', err);
    }
}
```

## Promise.all 和 Promise.race

### Promise.all

Promise.all 可以并行执行多个异步操作：

```javascript
async function readAllFiles() {
    const [data1, data2, data3] = await Promise.all([
        fs.promises.readFile('file1.txt', 'utf8'),
        fs.promises.readFile('file2.txt', 'utf8'),
        fs.promises.readFile('file3.txt', 'utf8')
    ]);
    console.log(data1 + data2 + data3);
}
```

### Promise.race

Promise.race 返回第一个完成的 Promise：

```javascript
async function raceExample() {
    const result = await Promise.race([
        fetch('https://api.example.com/fast'),
        new Promise((_, reject) => 
            setTimeout(() => reject(new Error('Timeout')), 3000)
        )
    ]);
    console.log(result);
}
```

### Promise.allSettled

Promise.allSettled 等待所有 Promise 完成，无论成功或失败：

```javascript
async function allSettledExample() {
    const results = await Promise.allSettled([
        fs.promises.readFile('file1.txt', 'utf8'),
        fs.promises.readFile('nonexistent.txt', 'utf8'),
        fs.promises.readFile('file3.txt', 'utf8')
    ]);
    
    results.forEach((result, index) => {
        if (result.status === 'fulfilled') {
            console.log(`File ${index + 1}:`, result.value);
        } else {
            console.log(`File ${index + 1} Error:`, result.reason);
        }
    });
}
```

## 错误处理

在异步编程中，错误处理非常重要：

```javascript
async function errorHandling() {
    try {
        const data = await fetchData();
        await saveData(data);
        await sendNotification();
    } catch (error) {
        console.error('发生错误:', error);
        await logError(error);
    } finally {
        await cleanup();
    }
}
```

## 并发控制

当有大量异步操作时，需要控制并发数量：

```javascript
async function concurrentLimit(tasks, limit) {
    const results = [];
    const executing = new Set();
    
    for (const task of tasks) {
        const promise = task().then(result => {
            results.push(result);
            executing.delete(promise);
        });
        
        executing.add(promise);
        
        if (executing.size >= limit) {
            await Promise.race(executing);
        }
    }
    
    await Promise.all(executing);
    return results;
}
```

## 选择合适的模式

- **简单的异步操作**：可以直接使用回调
- **需要链式处理**：使用 Promise
- **复杂的异步逻辑**：推荐使用 async/await
- **并行执行**：使用 Promise.all
- **需要控制并发**：实现并发限制

## 总结

Node.js 的异步编程模式经历了多次演进，每种模式都有其适用场景。async/await 是目前最推荐的方式，它让异步代码更加易读和维护。