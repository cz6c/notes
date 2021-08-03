***vant列表刷新的使用***

data中的数据

```js
      list: [],//列表数据
      page: 1,//页数
      total_pages: 0,//总页数
      loading: false,//控制是否触发上拉加载
      finished: false,//控制开启关闭上拉加载模式
      refreshing: false,//控制下拉刷新
      type:'', //请求数据类型，                          //多种类型列表来回切换时使用
```

页面创建调用``onRefresh()``方法，开启加载模式关闭上拉加载的触发把页数赋值1==>在``init()``中获取第一页的数据并开启上拉加载的触发==>此时上拉会触发``onLoad()``页数+1，获取下一页的数据==>当页数大于总页数的关闭加载模式

当下拉刷新时``refreshing=true``并触发``onRefresh()``==>在``init()``中先列表数据赋空，再``refreshing=false``

```js
  created() {
     this.onRefresh();
  },

  methods:{
 //下拉刷新初始化
    onRefresh() {
      // 开启加载模式
      this.finished = false;
      // 关闭上拉加载的触发
      this.loading = true;
      //把页面初始化到1
      this.page = 1;
      //获取列表数据
      this.init();
    },

 //上拉加载
    onLoad() {
      //页数加1
    if (this.page < this.total_pages) {
        this.page++;
        this.init();
      }
    },

 //获取列表数据
    init() {
       //定义query请求参数，类型，页数等
      const params = { type: this.type, page: this.page };
      //请求数据
      orderList(params).then((res) => {
        //下拉刷新开启时，把列表数据赋值空，关闭下拉刷新
        if (this.refreshing) {
          this.list = [];
          this.refreshing = false;
        }
        //把上次请求和本次请求的两数组合并到一个数组
        this.list = this.list.concat(res.data);
        //列表重新赋值后开启上拉加载的触发
        this.loading = false;
        //如果加载的页数已经超过总页数时关闭加载模式
        this.total_pages = res.meta.pagination.total_pages;
        if (this.page >= this.total_pages) {
          this.finished = true;
        }
      });
    }
  }  
```

当切换类型时，手动``refreshing = true;``调用``onRefresh()``重复下拉刷新的操作

```js
  //请求不同状态的数据                                    //多种类型列表来回切换时使用
    onClick(index) {
      //改变状态
      this.type = index + 1;
      //刷新数据
      this.refreshing = true;
      this.onRefresh();
    },
```