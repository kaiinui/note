beego Web Application Framework
===

http://beego.me/

すごくアグレッシブで面白い感じの Go WAF だった。

Rails を Go にまるまるポート
---

以下の要素がサポートされている

1. Routing (namespace 区切って出来る。あるいは **Annotation** で Controller Action に紐づけた Routing が定義出来る)
2. Model
  1. ORM 
  2. Validation
  3. Callbacks
3. Controller
4. Filter (`before_filter`)
5. Security (CSRF 等)
6. HTTP <=> Controller のアレコレ全部
7. i18n

Rails で出来ることはおおよそ全て出来るっぽい

Model とかは struct tag を宣言的に書ける要素として使ってる。
黒魔術っぽくて、色々大丈夫かな〜と思った。

Automated API document
---

```go
// @Title Get Product list
// @Description Get Product list by some info
// @Success 200 {object} models.ZDTProduct.ProductList
// @Param   category_id     query   int false       "category id"
// @Param   brand_id    query   int false       "brand id"
// @Param   query   query   string  false       "query of search"
// @Param   segment query   string  false       "segment"
// @Param   sort    query   string  false       "sort option"
// @Param   dir     query   string  false       "direction asc or desc"
// @Param   offset  query   int     false       "offset"
// @Param   limit   query   int     false       "count limit"
// @Param   price           query   float       false       "price"
// @Param   special_price   query   bool        false       "whether this is special price"
// @Param   size            query   string      false       "size filter"
// @Param   color           query   string      false       "color filter"
// @Param   format          query   bool        false       "choose return format"
// @Failure 400 no enough input
// @Failure 500 get products common error
// @router /products [get]
func (c *CMSController) Product() {

}
```

こんな感じに Annotation を書きまくることによって、swagger doc を自動生成することが出来る。
注目すべきはこれがフレームワークレベルでサポートされていること。

昨今の API の隆盛があるので良い判断だと思った。

ただし、Go は Annotation を言語レベルでサポートしていないので、IDE の恩恵を受けられず少し辛いと思った。

他
---

色々あった。ホントに色々ある。
