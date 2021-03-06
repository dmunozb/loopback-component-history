# loopback-component-history

[![Build Status](https://travis-ci.com/loopback4/loopback-component-history.svg?branch=master)](https://travis-ci.com/loopback4/loopback-component-history)

Saving history of `Create`, `Update`, `Delete` of a table sometimes is a big problem in data model design level.

A good approach for saving history is about adding some columns to your tables:

1. `UID`: A unique record identifier of the history
2. `BeginDate`: Record creation date
3. `EndDate`: Record deletion date
4. `ID`: History of one data is accessable using their same ID

-   Per every `Create` we will create a new record in table
-   Per every `Update` we will invalid last history, create new record
-   Per every `Delete` we will invalid last history

Now, using this simple extension you can add all history features to your models and repositories.

---

## Installation

```bash
npm i --save loopback-component-history
```

---

## Usage

### History Model

1. Change your model parent class from `Entity` to `HistoryEntity`
2. Remove `id` property from your model declaration

#### Example

Change your model from:

```ts
@model()
export class User extends Entity {
    @property({
        type: "string",
        unique: true,
        id: true,
    })
    id: string;

    @property({
        type: "string",
        default: "",
    })
    username: string;

    constructor(data?: Partial<User>) {
        super(data);
    }
}
```

To:

```ts
import { HistoryEntity } from "loopback-component-history";

@model()
export class User extends HistoryEntity {
    @property({
        type: "string",
        default: "",
    })
    username: string;

    constructor(data?: Partial<User>) {
        super(data);
    }
}
```

---

#### Tip

Don't use `unique` indexes in your models, instead add `unique` property to model definition

Convert your model from:

```ts
export class User extends Entity {
    @property({
        type: "string",
        required: true,
        index: {
            unique: true,
        },
    })
    username: string;
}
```

To:

```ts
export class User extends HistoryEntity {
    @property({
        type: "string",
        required: true,
        unique: true,
    })
    username: string;
}
```

---

### History Repository Mixin

Change your repository parent class from `DefaultCrudRepository` to `HistoryRepositoryMixin()()`

#### Example

Change your repository from:

```ts
export class UserRepository extends DefaultCrudRepository<
    User,
    typeof User.prototype.id,
    UserRelations
> {
    // ...
}
```

To:

```ts
import { HistoryRepositoryMixin } from "loopback-component-history";

export class UserRepository extends HistoryRepositoryMixin<
    User,
    UserRelations
>()() {
    // ...
}
```

---

## Contributions

-   [KoLiBer](https://www.linkedin.com/in/mohammad-hosein-nemati-665b1813b/)

## License

This project is licensed under the [MIT license](LICENSE).  
Copyright (c) KoLiBer (koliberr136a1@gmail.com)
