<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li @click="_selectMenu(index,$event)" v-for="(item,index) in goods" class="menu-item border-1px" :class="{'active':activeIndex === index}">
          <span class="text">
            <span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="goods-wrapper" ref="goodsWrapper">
      <ul>
        <li v-for="item in goods" class="goods-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border-1px">
                <div class="icon">
                  <img width="57" height="57" :src="food.icon">
                </div>
                <div class="content">
                  <h1 class="name">{{food.name}}</h1>
                  <p class="description">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span class="rating">好评{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <v-cartcontrol @cart-add="_cartAdd" :food="food"></v-cartcontrol>
                  </div>
                </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <v-shopcart ref="shopcart" :select-foods="selectFoods" :minPrice="seller.minPrice" :deliveryPrice="seller.deliveryPrice"></v-shopcart>
  </div>
</template>
<script type="text/ecmascript-6">
  import BScroll from "better-scroll";
  import shopcart from "../shopcart/shopcart";
  import cartcontrol from "../cartcontrol/cartcontrol";

  export default {
    props: {
      seller : {
        type : Object
      }
    },
    data() {
      return {
          goods : [
            {
              "name": "热销榜",
              "type": -1,
              "foods": [
                {
                  "name": "皮蛋瘦肉粥",
                  "price": 10,
                  "oldPrice": "",
                  "description": "咸粥",
                  "sellCount": 229,
                  "rating": 100,
                  "info": "一碗皮蛋瘦肉粥，总是我到粥店时的不二之选。香浓软滑，饱腹暖心，皮蛋的Q弹与瘦肉的滑嫩伴着粥香溢于满口，让人喝这样的一碗粥也觉得心满意足",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "很喜欢的粥",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 1,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "扁豆焖面",
                  "price": 14,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 188,
                  "rating": 96,
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 1,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "info": "",
                  "icon": "http://fuss10.elemecdn.com/c/6b/29e3d29b0db63d36f7c500bca31d8jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/c/6b/29e3d29b0db63d36f7c500bca31d8jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "葱花饼",
                  "price": 10,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 124,
                  "rating": 85,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 1,
                      "text": "没啥味道",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 1,
                      "text": "很一般啊",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/f/28/a51e7b18751bcdf871648a23fd3b4jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/f/28/a51e7b18751bcdf871648a23fd3b4jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "牛肉馅饼",
                  "price": 14,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 114,
                  "rating": 91,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 1,
                      "text": "难吃不推荐",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/d/b9/bcab0e8ad97758e65ae5a62b2664ejpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/d/b9/bcab0e8ad97758e65ae5a62b2664ejpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "招牌猪肉白菜锅贴/10个",
                  "price": 17,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 101,
                  "rating": 78,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 1,
                      "text": "不脆,不好吃",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/7/72/9a580c1462ca1e4d3c07e112bc035jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/7/72/9a580c1462ca1e4d3c07e112bc035jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "南瓜粥",
                  "price": 9,
                  "oldPrice": "",
                  "description": "甜粥",
                  "sellCount": 91,
                  "rating": 100,
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/8/a6/453f65f16b1391942af11511b7a90jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/8/a6/453f65f16b1391942af11511b7a90jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "红豆薏米美肤粥",
                  "price": 12,
                  "oldPrice": "",
                  "description": "甜粥",
                  "sellCount": 86,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/d/22/260bd78ee6ac6051136c5447fe307jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/d/22/260bd78ee6ac6051136c5447fe307jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "八宝酱菜",
                  "price": 4,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 84,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "红枣山药糙米粥",
                  "price": 10,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 81,
                  "rating": 91,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "糊塌子",
                  "price": 10,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 80,
                  "rating": 93,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/0/05/097a2a59fd2a2292d08067e16380cjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/0/05/097a2a59fd2a2292d08067e16380cjpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            },
            {
              "name": "单人精彩套餐",
              "type": 2,
              "foods": [
                {
                  "name": "红枣山药粥套餐",
                  "price": 29,
                  "oldPrice": 36,
                  "description": "红枣山药糙米粥,素材包,爽口莴笋丝,四川泡菜或八宝酱菜,配菜可备注",
                  "sellCount": 17,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/6/72/cb844f0bb60c502c6d5c05e0bddf5jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/6/72/cb844f0bb60c502c6d5c05e0bddf5jpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            },
            {
              "name": "冰爽饮品限时特惠",
              "type": 1,
              "foods": [
                {
                  "name": "VC无限橙果汁",
                  "price": 8,
                  "oldPrice": 10,
                  "description": "",
                  "sellCount": 15,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "还可以",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/e/c6/f348e811772016ae24e968238bcbfjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/e/c6/f348e811772016ae24e968238bcbfjpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            },
            {
              "name": "精选热菜",
              "type": -1,
              "foods": [
                {
                  "name": "娃娃菜炖豆腐",
                  "price": 17,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 43,
                  "rating": 92,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "菜量还可以,味道还可以",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/d/2d/b1eb45b305635d9dd04ddf157165fjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/d/2d/b1eb45b305635d9dd04ddf157165fjpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "手撕包菜",
                  "price": 16,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 29,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/9/c6/f3bc84468820121112e79583c24efjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/9/c6/f3bc84468820121112e79583c24efjpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "香酥黄金鱼/3条",
                  "price": 11,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 15,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/4/e7/8277a6a2ea0a2e97710290499fc41jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/4/e7/8277a6a2ea0a2e97710290499fc41jpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            },
            {
              "name": "爽口凉菜",
              "type": -1,
              "foods": [
                {
                  "name": "八宝酱菜",
                  "price": 4,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 84,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "拍黄瓜",
                  "price": 9,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 28,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/6/54/f654985b4e185f06eb07f8fa2b2e8jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/6/54/f654985b4e185f06eb07f8fa2b2e8jpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            },
            {
              "name": "精选套餐",
              "type": -1,
              "foods": [
                {
                  "name": "红豆薏米粥套餐",
                  "price": 37,
                  "oldPrice": "",
                  "description": "红豆薏米粥,三鲜干蒸烧卖,拍黄瓜",
                  "sellCount": 3,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/f/49/27f26ed00c025b2200a9ccbb7e67ejpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/f/49/27f26ed00c025b2200a9ccbb7e67ejpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "皮蛋瘦肉粥套餐",
                  "price": 31,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 12,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/8/96/f444a8087f0e940ef264617f9d98ajpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/8/96/f444a8087f0e940ef264617f9d98ajpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            },
            {
              "name": "果拼果汁",
              "type": -1,
              "foods": [
                {
                  "name": "蜜瓜圣女萝莉杯",
                  "price": 6,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 1,
                  "rating": "",
                  "info": "",
                  "ratings": [],
                  "icon": "http://fuss10.elemecdn.com/b/5f/b3b04c259d5ec9fa52e1856ee50dajpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/b/5f/b3b04c259d5ec9fa52e1856ee50dajpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "加多宝",
                  "price": 6,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 7,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/b/9f/5e6c99c593cf65229225c5661bcdejpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/b/9f/5e6c99c593cf65229225c5661bcdejpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "VC无限橙果汁",
                  "price": 8,
                  "oldPrice": 10,
                  "description": "",
                  "sellCount": 15,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "还可以",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/e/c6/f348e811772016ae24e968238bcbfjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/e/c6/f348e811772016ae24e968238bcbfjpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            },
            {
              "name": "小吃主食",
              "type": -1,
              "foods": [
                {
                  "name": "扁豆焖面",
                  "price": 14,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 188,
                  "rating": 96,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 1,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/c/6b/29e3d29b0db63d36f7c500bca31d8jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/c/6b/29e3d29b0db63d36f7c500bca31d8jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "葱花饼",
                  "price": 10,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 124,
                  "rating": 85,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 1,
                      "text": "没啥味道",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 1,
                      "text": "很一般啊",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/f/28/a51e7b18751bcdf871648a23fd3b4jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/f/28/a51e7b18751bcdf871648a23fd3b4jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "牛肉馅饼",
                  "price": 14,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 114,
                  "rating": 91,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 1,
                      "text": "难吃不推荐",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/d/b9/bcab0e8ad97758e65ae5a62b2664ejpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/d/b9/bcab0e8ad97758e65ae5a62b2664ejpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "招牌猪肉白菜锅贴/10个",
                  "price": 17,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 101,
                  "rating": 78,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 1,
                      "text": "不脆,不好吃",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/7/72/9a580c1462ca1e4d3c07e112bc035jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/7/72/9a580c1462ca1e4d3c07e112bc035jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "糊塌子",
                  "price": 10,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 80,
                  "rating": 93,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/0/05/097a2a59fd2a2292d08067e16380cjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/0/05/097a2a59fd2a2292d08067e16380cjpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            },
            {
              "name": "特色粥品",
              "type": -1,
              "foods": [
                {
                  "name": "皮蛋瘦肉粥",
                  "price": 10,
                  "oldPrice": "",
                  "description": "咸粥",
                  "sellCount": 229,
                  "rating": 100,
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "很喜欢的粥",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 1,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "南瓜粥",
                  "price": 9,
                  "oldPrice": "",
                  "description": "甜粥",
                  "sellCount": 91,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/8/a6/453f65f16b1391942af11511b7a90jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/8/a6/453f65f16b1391942af11511b7a90jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "红豆薏米美肤粥",
                  "price": 12,
                  "oldPrice": "",
                  "description": "甜粥",
                  "sellCount": 86,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/d/22/260bd78ee6ac6051136c5447fe307jpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/d/22/260bd78ee6ac6051136c5447fe307jpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "红枣山药糙米粥",
                  "price": 10,
                  "oldPrice": "",
                  "description": "",
                  "sellCount": 81,
                  "rating": 91,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "鲜蔬菌菇粥",
                  "price": 11,
                  "oldPrice": "",
                  "description": "咸粥",
                  "sellCount": 56,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": ""
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/e/a3/5317c68dd618929b6ac05804e429ajpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/e/a3/5317c68dd618929b6ac05804e429ajpeg.jpeg?imageView2/1/w/750/h/750"
                },
                {
                  "name": "田园蔬菜粥",
                  "price": 10,
                  "oldPrice": "",
                  "description": "咸粥",
                  "sellCount": 33,
                  "rating": 100,
                  "info": "",
                  "ratings": [
                    {
                      "username": "3******c",
                      "rateTime": 1469281964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "2******3",
                      "rateTime": 1469271264000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    },
                    {
                      "username": "3******b",
                      "rateTime": 1469261964000,
                      "rateType": 0,
                      "text": "",
                      "avatar": "http://static.galileo.xiaojukeji.com/static/tms/default_header.png"
                    }
                  ],
                  "icon": "http://fuss10.elemecdn.com/a/94/7371083792c19df00e546b29e344cjpeg.jpeg?imageView2/1/w/114/h/114",
                  "image": "http://fuss10.elemecdn.com/a/94/7371083792c19df00e546b29e344cjpeg.jpeg?imageView2/1/w/750/h/750"
                }
              ]
            }
          ],
          listHeight: [],
          scrollY: 0
      };
    },
    computed : {
      activeIndex() {
          for(let i=0;i<this.listHeight.length;i++) {
            let height1 = this.listHeight[i];
            let height2 = this.listHeight[i+1];
            if(!height2 || this.scrollY >= height1 && this.scrollY < height2)
              return i;
          }
          return 0;
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((item) => {
          item.foods.forEach((food) => {
             if(food.count) {
                foods.push(food)
             }
          })
        });
        return foods;
      }
    },
    methods : {
      _initScroll() {
        this.meunScroll = new BScroll(this.$refs.menuWrapper,{
          click : true
        });
        this.goodsScroll = new BScroll(this.$refs.goodsWrapper,{
          click : true,
          probeType : 3
        });
        this.goodsScroll.on('scroll',(pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        });
      },
      _selectMenu(index,event) {
        if(!event._constructed){
          return
        };
        let goodsList = this.$refs.goodsWrapper.getElementsByClassName('goods-list-hook');
        let el = goodsList[index];
        this.goodsScroll.scrollToElement(el, 300);
      },
      _getScroll() {
        let lis = this.$refs.goodsWrapper.getElementsByClassName('goods-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for(var i=0;i<lis.length;i++) {
          let obj = lis[i];
          height += obj.clientHeight;
          this.listHeight.push(height);
        }
      },
      _cartAdd(target) {
        this.$refs.shopcart.drop(target);
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$nextTick(() => {
        this._initScroll();
        this._getScroll();
      });
    },
    components: {
      'v-shopcart' : shopcart,
      'v-cartcontrol' : cartcontrol
    }
  };
</script>
<style lang="stylus" ref="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"

  .goods
    display:flex
    width: 100%
    position: absolute
    top: 174px
    bottom:46px
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        display: table
        height: 54px
        width: 56px
        line-height:14px
        font-size: 12px
        padding: 0 12px
        &.active
          position:relative
          margin-top: -1px
          z-index: 100
          background: #fff
          font-weight: 700
          .text
           border-none()
        .text
          display: table-cell
          vertical-align:middle
          border-1px(rgba(7,17,27,0.1))
          .icon
            display: inline-block
            height: 12px
            width:12px
            background-repeat: no-repeat
            background-size: 12px 12px
            &.decrease
              bg-image('decrease_3')
            &.discount
              bg-image('discount_3')
            &.guarantee
              bg-image('guarantee_3')
            &.invoice
              bg-image('invoice_3')
            &.special
              bg-image('special_3')
    .goods-wrapper
      flex: 1
      .title
        padding-left: 10px
        height: 26px
        line-height: 26px
        border-left: 2px solid #d9dde1
        background: #f3f5f7
        color: rgb(147,153,159)
        font-size: 12px
      .food-item
        display:flex
        margin: 18px
        padding-bottom: 18px
        border-1px(rgba(7,17,27,0.1))
        &:last-child
          border-none()
          padding-bottom: 0
        .icon
          flex: 0 0 57px
          margin-right:10px
        .content
          flex: 1
          position: relative
          .name
            margin: 2px 0 8px 0
            font-size: 14px
            line-height: 14px
            height: 14px
            color: rgb(7,17,27)
          .description,.extra
            font-size: 10px
            line-height: 14px
            color: rgb(147,153,159)
          .description
            margin-bottom: 8px
          .extra
            .count
              margin-right: 12px
          .price
            font-weight: 700
            line-height: 24px
            .now
              font-size: 14px
              margin-right: 12px
              color: rgb(1240,20,20)
            .old
              font-size: 10px
              text-decoration:line-through

          .cartcontrol-wrapper
            position: absolute
            bottom: 0
            right: 0

</style>
