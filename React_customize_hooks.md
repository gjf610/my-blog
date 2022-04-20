## React 自定义Hooks

现在我用精卫记账来演示自定义Hooks的使用，[预览](https://gjf610.github.io/JingWei-morney/dist)

有三个界面，分别是记账、标签和统计，这次我们的主角是标签页。

用了React的几个Hook Api 以及自定义Hook,直接上代码。

* 首先理一下逻辑：标签页 (tags.tsx) 里面 import 了 hook/useTags；
* useTags里 import 了 useEffect, useState ，其他都无关紧要；
```js
import { createId } from "../lib/createId";
import { useEffect, useState } from "react";
import { useUpdate } from "./useUpdate";

const useTags = () => {
  const [tags, setTags] = useState<{ id: number, name: string }[]>([])
  useEffect(() => {
    let localTags = JSON.parse(window.localStorage.getItem('tags') || '[]')
    if (localTags.length === 0) {
      localTags = [
        { id: createId(), name: '衣' },
        { id: createId(), name: '食' },
        { id: createId(), name: '住' },
        { id: createId(), name: '行' }
      ]
    }
    setTags(localTags)
  }, [])// 组件挂载时执行
}
```
在useTags函数里
* 第一行 析构赋值<code>useState</code> 有两个参数[tags, setTags] 声明好类型<id:string; name:string>初始值为[]
* 第二行使用Hook<code>useEffect</code>， 写一个箭头函数
* 第三行 let 一个 变量 localTags ，用JSON.parse localStorage 里的 'tags'(字符串)转为对象，要不然会报错
* 第四行if else 表达式，localStorage没数据的话就创建'衣食住行'，这是为了保证用户首次进入有tag可选。
* 最后 setTags就是把localTags 导入,deps为[]增加代码可维护性。

从中我们可以得出之前[总结的一些结论](./React_hook.md#useEffect（副作用）)，如果用class来写会使代码变得冗长重复，不利于维护。

接下来是自定义Hook : useUpdate
```js
import { useEffect, useRef } from "react"
const useUpdate = (fn: () => void, dependency: any[]) => {
  const count = useRef(0)
  useEffect(() => {
    count.current += 1
  })
  useEffect(() => {
    if (count.current > 1) {
      fn()
    }
  }, [fn, dependency])
}
export { useUpdate }
```
自定义Hook强大之处主要体现在其可选择性之广
```js
const useTags = () => {
  const [tags, setTags] = useState<{ id: number, name: string }[]>([])
  useEffect(() => {
    let localTags = JSON.parse(window.localStorage.getItem('tags') || '[]')
    if (localTags.length === 0) {
      localTags = [
        { id: createId(), name: '衣' },
        { id: createId(), name: '食' },
        { id: createId(), name: '住' },
        { id: createId(), name: '行' }
      ]
    }
    setTags(localTags)
  }, [])// 组件挂载时执行

  useUpdate(() => {
    window.localStorage.setItem('tags', JSON.stringify(tags))
  }, tags)
  const findTag = (id: number) => tags.filter(tag => tag.id === id)[0]
  const findTagIndex = (id: number) => {
    let result = -1
    for (let i = 0; i < tags.length; i++) {
      if (tags[i].id === id) {
        result = i;
        break;
      }
    }
    return result
  }
  const addTag = () => {
    console.log('addTag')
    const tagName = window.prompt('新标签的名称为');
    if (tagName !== null && tagName !== '') {
      // TODO
      setTags([...tags, { id: createId(), name: tagName }])
    }
  }
  const updateTag = (id: number, { name }: { name: string }) => {
    setTags(tags.map(tag => tag.id === id ? { id, name } : tag));
  }
  const deleteTag = (id: number) => {
    setTags(tags.filter(tag => tag.id !== id))
  }
  const getName = (id: number) => {
    const tag = tags.filter(t => t.id === id)[0]
    return tag ? tag.name : ''
  }
  return { tags, getName, setTags, findTag, findTagIndex, addTag, updateTag, deleteTag }
}

export { useTags }
```

* 自定义 Hook 是一个函数，其名称以 “use” 开头，函数内部可以调用其他的 Hook。

* 整体逻辑为： useState 里引用了 自带的useEffect 和自定义useUpdate，

* 封装了 findTag, updateTag, deleteTag, addTag, getName等Api。

* 避免了代码过于冗长而造成的杂乱，以及后期维护的困难所造成的问题。