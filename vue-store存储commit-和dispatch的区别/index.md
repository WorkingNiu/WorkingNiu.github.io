# vuex存储commit和dispatch的区别


项目中用到Vuex,一点个人理解

<!--more-->

**dispatch：含有异步操作**

存储:

```javascript
this.$store.dispatch('setTargetUser',friend);
```
取值:
```javascript
this.$store.getters.targetUser;
```

**commit：同步操作**

存储:

```javascript
this.$store.commit('setTargetUser',friend);
```
取值:
```javascript
this.$store.state.setTargetUser;
```
