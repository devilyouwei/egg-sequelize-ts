# egg-sequelize-typescript

## 更改

npmjs 上的的 egg-typescript 基本都停止维护了

本项目已更新了所有的 dependencies 包，适配了最新的 typescript 版 egg.js

此插件已在生产项目中得到实践：[https://github.com/uni-openai/uniai-maas](https://github.com/uni-openai/uniai-maas。

## 目的

能让使用 `typescript` 编写的 egg.js 项目中能够使用 sequelize 方法，并同时得到 egg.js 所赋予的功能。

## 安装

```bash
npm i --save egg-sequelize-typescript
# or
yarn add egg-sequelize-typescript
```

## 配置

- Enable plugin in `config/plugin.js`
- 在 `config/plugin.js` 文件中引入 `egg-sequelize-typescript` 组件

```js
exports.sequelize = {
  enable: true,
  package: "egg-sequelize-typescript",
};
```

- Edit your own configurations in `conif/config.{env}.js`
  在 `conif/config.{env}.js` 中编写 sequelize 配置

```js
config.sequelize = {
  dialect: "mysql",
  host: "127.0.0.1",
  port: 3306,
  database: "database",
};
```

## 例子

分别以 `model/user.ts` 和 `service/user.js` 举例说明

> note 注意我们都是从 `sequelize-typescript` 中`import`类名，方法，属性等。

```ts
// app/model/user.ts

/**
 * @desc 用户表
 */
import {
  AutoIncrement,
  Column,
  DataType,
  Model,
  PrimaryKey,
  Table,
} from "sequelize-typescript";
@Table({
  modelName: "user",
})
export class User extends Model<User> {
  @PrimaryKey
  @AutoIncrement
  @Column({
    type: DataType.INTEGER(11),
    comment: "用户ID",
    comment: "user id",
  })
  id: number;

  @Column({
    comment: "用户姓名",
  })
  name: string;

  @Column({
    comment: "用户邮箱",
  })
  email: string;

  @Column({
    comment: "用户手机号码",
  })
  phone: string;

  @Column({
    field: "created_at",
  })
  createdAt: Date;

  @Column({
    field: "updated_at",
  })
  updatedAt: Date;
}
export default () => User;
```

```ts
// app/service/user.ts
import { Service } from "egg";
import { Sequelize } from "sequelize-typescript";

class UserService extends Service {
  async index() {
    const { or } = Sequelize.Op;
    const users = await this.ctx.model.User.findOne({
      where: {
        [or]: [{ name, phone }, { id }],
      },
    });
    this.ctx.body = users;
  }
}
```

刷新下`types`

```bash
yarn ets
```
