##vue3新增api-setup

### ref,reactive,toRef,toRefs的用法
写法一
```$xslt
<script setup>
    import componentA from './components'
    import { ref, reactive } from 'vue'
    
    export const a = ref(0)
    export const add = () => a.value++
    export const b = reactive({num: 10})
</script>
```

* 引入的组件无需注册即可使用
* 组件在编译的过程中代码运行的上下是 setup() 函数中。所有ES模块导出都被认为是暴露给上下文的值，并包含在 setup() 返回对象中。

写法二
```$xslt
<script>
    import componentA from './components'
    import { ref, reactive, toRef, toRefs } from 'vue'

    export default {
        components: {componentA}

        props: {
            msg: String,
            item: Object
        }

        setup (props) {
            const num = ref(0)
            const sta = reactive({cou: 99})
            const Add = () => {
              sta.cou++
            }
            const newMsg = toRef(props, 'msg')
            console.log(newMsg.value)

            const { newMsg2 } = toRefs(props)
            console.log(newMsg2.value)

        
            return {
              num,
              sta,
              Add,
              newMsg,
              newMsg2
            }
        }
    }
    
</script>
```

* ref用于简单类型的数据创建响应式对象，在setup里读取时需要.value取值
* reactive用于为对象添加响应式状态，获取值是直接获取
* toRef用于为源响应式对象上的属性新建ref，从而保持对其源对象属性的响应式连接.获取值时.value，是对原有数据的引用
* toRef是对某个属性赋值，toRefs是自动赋值
