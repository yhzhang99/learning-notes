# jsx
组件属性props，普通html属性attrs，Dom属性domProps

## 基本使用

### 父
``` javascript
<script>
import TodoItem from './TodoItem.vue';
const ButtonCounter = {
  name: 'button-counter',
  props: ['count'],
  methods: {
    onClick() {
      this.$emit('change', this.count + 1);
    },
  },
  render() {
    return (
      <button onClick={this.onClick}>You clicked me {this.count} times.</button>
    );
  },
};

const MySlot = {
  render(h) {
    return h('div', [
      h('header', [this.$slots.header]),
      h('main', [this.$slots.header]),
      h('footer', [this.$slots.footer]),
    ]);
  },
};

const MySlot2 = {
  data() {
    return { user: 'John', content: 'vue', copytight: 'CopyRight' };
  },
  render() {
    return (
      <div>
        <header>{this.$scopedSlots.header({ user: this.user })}</header>
        <main>{this.$scopedSlots.default({ content: this.content })}</main>
        <footer>
          {this.$scopedSlots.footer({ copytight: this.copytight })}
        </footer>
      </div>
    );
  },
};

export default {
  name: 'button-counter-container',
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    onChange(val) {
      this.count = val;
    },
  },
  components: {
    TodoItem,
  },
  render() {
    return (
      <div>
        <p>基本使用</p>
        {/*v-bind*/}
        <input placeholder={this.count} />
        <p>{this.msg}</p>
        {/* 自定义组件 */}
        <ButtonCounter
          style={{ marginTop: '10px' }}
          count={this.count}
          type="button"
          onChange={this.onChange}
        />
        <ButtonCounter
          style={{ marginTop: '10px' }}
          count={this.count}
          type="button"
          domPropsInnerHTML={`hello ${this.count}.`}
          onChange={this.onChange}
        />
        {/* 具名插槽 */}
        <todo-item>
          <header slot="header">header</header>
          <header slot="content">content</header>
          <footer slot="footer">footer</footer>
        </todo-item>
        {/* 插槽 */}
        <MySlot>
          <template slot="header">hello world</template>
          children node
          <div slot="footer">this is footer</div>
        </MySlot>
        {/* 作用域插槽 */}
        <MySlot2
          scopedSlots={{
            header: (props) => `hello, ${props.user}`,
          }}
        >
          children node
          <div slot="footer">this is footer</div>
        </MySlot2>
      </div>
    );
  },
};
</script>
```

### 子
``` javascript
<script>
export default {
  name: 'TodoItem',
  data() {
    return {};
  },
  render() {
    return (
      <div>
        {/* 具名插槽 */}
        <p>我是自定义组件</p>
        {this.$slots.header}
        <p>我是自定义组件</p>
        {this.$slots.content}
        <p>我是自定义组件</p>
        {this.$slots.footer}
      </div>
    );
  },
};
</script>
```