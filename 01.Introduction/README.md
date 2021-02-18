# TypeScriptの定義

[公式サイト](https://www.typescriptlang.org/)に掲載されているように

1. **型付き**拡張された`JavaScript`、実は`TypeScript`が`JavaScript`の**スーパーセット**です。
2. `JavaScript`を稼働出来る環境では`TypeScript`も稼働できます。要するに、`TypeScript`をコンパイルして`JavaScript`に変換します。

## 型付き

全ての変数に型の指定は必須。

例：`JavaScript`だと下記の書き方は問題ないですが、`TypeScript`だとエラーが起こります。

```javascript
let v = 1000;
v = 'FGL';
```

## JavaScriptのスーパーセット

ES6の全ての便利機能を使用することができます。ここでいくつかの便利機能を紹介します。

### [1. アロー関数](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

`python`に似ていて、より使いやすい`lambda`関数のような関数です。

```python
sum = lambda x, y: x + y
print(sum(1, 2))
```

`ES6`の新規機能で、通常の function 式をより短く記述できる代替構文です。また、this, arguments, super, new.target を束縛しません。アロー関数式は、メソッドでない関数に最適です。

```javascript
const sum = (x, y) => {
return x + y;
}
console.log(sum(1, 2));
```

上記の関数は下記と同じです。

```javascript
function sum(x, y) {
  return x + y;
}
console.log(sum(1, 2));
```

アロー関数の書き方：`=>`の左はパラメーター、右は関数の本体

- パラメータの書き方：

  ```javascript
  // 1. パラメータなし：()は必須
  () =>

  // 2. パラメータが一つしかない：()は必須ではない
  (a) =>
  a =>

  // 3. パラーメータが複数の場合：()は必須
  (a, b, c) =>
  ```

- 関数本体の書き方：

  ```javascript
  // 1. 関数本体がワンステートメントの場合、{}は不要。戻り値がある場合、returnは省略
  => console.log('Hello World');
  => x + y;

  // 2. 関数本体がワンステートメントであり、オブジェクトを戻したら、()が必要
  => ({ name: 'Kondo', role: 'Key Man' });

  //3. 関数本体が複数のステートメントの場合、{}は必須
  => {
       const a = 1;
       const b = 1;
       const c = 3;
       return a + b + c;
  }
  ```

**注意：アロー関数の書き方の組み合わせは多いため、開発チームで統一しないといけません。**

FGL三フィーチャの場合（Airbnbルール）：

- パラメータは必ず`()`をつけること。
- 関数本体は上記のルールを適応すること。

利用シーン：よくコールバック関数として使われています。

   ```javascript
   const sortedArray = [3, 4, 1, 6, 8].sort((a, b) => b - a);
   console.log(sortedArray);
   ```

### [2. スプレッドオペレータ](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

`...`のような、オブジェクトとのトラバーサル属性を展開する演算子です。例えば：

```javascript
const sum = (x, y, z) => x + y + z;
console.log(sum(...[1, 2, 3]));
```

スプレッドオペレータはいろいろ便利な用途を持っています。

- 配列の合併：

  ```javascript
  let a = [1, 2];
  let b = [3, 4];
  console.log([...a, ...b]);
  ```

- 配列のディープコピー

  `JavaScript`の配列は**参照渡し**のため（C言語のポインタと類似）、直接`array1 = array2`のように複製できません。

  ```javascript
  const srcArray = [1, 2, 3, 4];
  const tgtArray = srcArray;
  console.log('Before change: ', tgtArray);

  srcArray[0] = 0
  console.log('After change: ', tgtArray);
  ```

  ディープコピーの実現手段たくさんありますが、...で一番簡単です。

  ```javascript
  const srcArray = [1, 2, 3, 4];
  const tgtArray = [...srcArray];
  console.log('Before change: ', tgtArray);

  srcArray[0] = 0
  console.log('After change: ', tgtArray);
  ```

- オブジェクトの合併：

  ```javascript
  const apiData = () => ({ floor: '1F', zone: 'Meeting Room' });
  let originalData = { building:  'ICC'};
  console.log({...apiData(), ...originalData});
  ```

- オブジェクトのディメンションリダクション：

  ```javascript
  const apiData = () => [
    {
      floor_name: 1,
      floor_config: {
        temperature: 32,
        square: 200,
        floor_id: 1,
      }
    },
    {
      floor_id: 2,
      floor_config: {
        temperature: 33,
        square: 190,
      }
    }
  ];

  // floor_configはデータ処理時に役に立たないので、削除したい
  const localData = apiData().map(e => (
  {
    floor_id: e.floor_id,
    ...e.floor_config,
  }));
  console.log(localData);
  ```

## [3. テンプレートリテラル](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Template_literals)

組み込み式を扱うことができる文字列リテラルです。複数行の文字列や文字列挿入機能を使用することができます。

```javascript
const str = `Kondo`;
console.log(str);
```

テンプレートリテラルは変数をいれることができます。

```javascript
const age = 20;
const str = `I am ${age} yeas old`;
console.log(str);
```

テンプレートリテラルはAPIの**Route**を作成するのに非常に便利です。
`/site/:site_id/building/:building_id/`を考えましょう。

- 従来の作り方：`+`を演算子でルートを結合します。

  ```javascript
  // 従来のやり方（文字列結合）
  const getBuilding = (siteId, buildingId) => {
  const route = '/site/' + siteId.toString() + '/building/' + buildingId.toString() + '/';
    console.log(route);
  }
  getBuilding(0, 0);
  ```

  その方法はみづらく、特に`/`漏れなどミスが発生しやすいでしょう。

- テンプレートリテラルででルートを結合します。

  ```javascript
  const getBuilding = (siteId, buildingId) => {
  const route = `/site/${siteId}/building/${buildingId}/`;
  console.log(route);
  }
  getBuilding(0, 0);
  ```

  テンプレートリテラルは、ルートと似ているため、間違いにくく、自然な感じです。

## [3. 分割代入](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

配列やオブジェクト個別変数を代入することができます。

- オブジェクト：オブジェクトのスキーマを使うことで分割します。

  注意することは、変数とオブジェクのスキーマの名前は必ず**統一**することです。

  ```javascript
  const getApi = () => (
    {
      id: 1,
      name: 'siteData',
      temperature: {
        cool: 30,
        heat: 24,
      }
    }
  )
  
  // nameが欲しければ：
  const { name } = getApi();
  console.log(name);
  
  //nameとcool欲しければ：
  const { id, temperature: { cool } } = getApi();
  console.log(id);
  console.log(cool);
  ```

- 関数のパラメータとしての運用：

  下記のリモートデータを要求する関数を考えましょう：

  ```javascript
  const getDevice =  (siteId, buildingId, floorId, zoneId, refId, unitId) => {
    console.log(`/site/${siteId}/building/${buildingId}/floor/${floorId}/zone/${zoneId}/ref/${refId}/unit/${unitId}`);
  }
  
  getDevice(1, 2, 1, 2, 0, 0);
  ```

  上記の関数はパラメーターが多く、順番も固定ですので、バグが発生しやすいでしょう。

  パラーメータをオブジェクトに収束することで、順番などを意識しなくてもパラメータを渡せます。

  ```javascript
  const getDevice = (deviceInfo) => {
  // 分割代入
    const { siteId, buildingId, floorId, zoneId, refId, unitId } = deviceInfo;
    console.log(`/site/${siteId}/building/${buildingId}/floor/${floorId}/zone/${zoneId}/ref/${refId}/unit/${unitId}`);
  }

  const deviceInfo = {
    siteId: 1,
    buildingId: 2,
    floorId: 1,
    zoneId: 2,
    refId: 0,
    unitId: 0,
  }

  getDevice(deviceInfo);
  ```

  この方法のように、わざわざオブジェクトを定義するのは面倒臭いため、渡すパラメータも分割しましょう。

   ```javascript
  const getDevice = (deviceInfo) => {
    // 分割代入
    const { siteId, buildingId, floorId, zoneId, refId, unitId } = deviceInfo;
    console.log(`/site/${siteId}/building/${buildingId}/floor/${floorId}/zone/${zoneId}/ref/${refId}/unit/${unitId}`);
   }

  getDevice({ siteId: 1, buildingId: 2, floorId: 1, zoneId: 2, refId: 0, unitId: 0 });
   ```

  関数の`deviceInfo`だけ中身がわからないので、さらに分割し、最終型は：

  ```javascript
  const getDevice = ({ siteId, buildingId, floorId, zoneId, refId, unitId }) => {
    console.log(`/site/${siteId}/building/${buildingId}/floor/${floorId}/zone/${zoneId}/ref/${refId}/unit/${unitId}`);
  }

  getDevice({ siteId: 1, buildingId: 2, floorId: 1, zoneId: 2, refId: 0, unitId: 0 });
  ```

  FGL三フィーチャーのルール（Airbnbルール）：関数のパラーメータが三個以上の場合はオブジェクト化をし、分割代入でスキーマを明記すること。

- 配列：配列のデータも分割できます。

  ```javascript
  const data = [1, 2, 3, 4, 5];
  //1番目と2番目のデータ
  const [e1, e2] = data;
  console.log(e1);
  console.log(e2);

  // 3番目と5番目のデータ
  const [, , e3, , e5] = data;
  console.log(e3);
  console.log(e5);

  // 1番目以外のデータ
  const [, ...left] = data
  console.log(left);
  ```

## コンパイル

ブラウザは`TypeScript`を直接認識しないため、コンパイルをして`JavaScript`に変換すると、ブラウザに認識されます。

```typescript
class LivingBeings {
    gender: string;
    constructor(gender: string){
        this.gender = gender;
    }
}

class Human extends LivingBeings {
    name: string;
    constructor(gender: string, name: string) {
        super(gender);
        this.name = name;
    }
}
```

コンパイルの結果

```javascript
"use strict";
var __extends = (this && this.__extends) || (function () {
    var extendStatics = function (d, b) {
        extendStatics = Object.setPrototypeOf ||
            ({ __proto__: [] } instanceof Array && function (d, b) { d.__proto__ = b; }) ||
            function (d, b) { for (var p in b) if (Object.prototype.hasOwnProperty.call(b, p)) d[p] = b[p]; };
        return extendStatics(d, b);
    };
    return function (d, b) {
        extendStatics(d, b);
        function __() { this.constructor = d; }
        d.prototype = b === null ? Object.create(b) : (__.prototype = b.prototype, new __());
    };
})();
var LivingBeings = /** @class */ (function () {
    function LivingBeings(gender) {
        this.gender = gender;
    }
    return LivingBeings;
}());
var Human = /** @class */ (function (_super) {
    __extends(Human, _super);
    function Human(gender, name) {
        var _this = _super.call(this, gender) || this;
        _this.name = name;
        return _this;
    }
    return Human;
}(LivingBeings));
```
