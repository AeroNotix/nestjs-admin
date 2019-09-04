<h1 align="center">NestJS Admin</h1>

<h3 align="center">
  A generic administration interface for TypeORM entities
</h3>
<p align="center">
  <a href="https://www.npmjs.com/package/nestjs-admin">
    <img src="https://img.shields.io/npm/v/nestjs-admin.svg" alt="NPM Version" />
  </a>
</p>

<div>
  <div align="center">
    <em>Proudly sponsored by <a href="https://www.theodo.co.uk/experts/node-js-fullstack-javascript">Theodo</a></em>
  </div>
  <div align="center">
    <a href="https://www.theodo.co.uk/experts/node-js-fullstack-javascript">
      <img src="./docs/assets/theodo.svg" width="120rem" alt="Theodo logo" />
    </a>
  </div>
</div>

<br />

---

## Description

Ready-to-use user interface for administrative activities. Allows to list, edit, create, delete entities.

This is heavily, heavily inspired by [Django admin](https://djangobook.com/mdj2-django-admin/), from the concept to the API.

> This is still very much a work in progress.
> Your help is more than welcome!

## Installation

Let's get you started with a minimal setup.

1. Add nestjs-admin to your dependencies:

```bash
yarn add nestjs-admin # With yarn
npm install nestjs-admin # With NPM
```

2. Then add the provided `DefaultAdminModule` to your app modules:

> Note that the DefaultAdminModule will introduce an AdminUser entity.
> If you want to be able to use your own User entity to authenticate to the admin interface,
> you'll have to write your own module instead.

```ts
// src/app.module.ts
import { Module } from '@nestjs/common'
import { DefaultAdminModule } from 'nestjs-admin'

@Module({
  imports: [TypeOrmModule.forRoot(), /* ... */, DefaultAdminModule],
  /* ... */,
})
export class AppModule {
  /* ... */
}
```

> Need customization? You can use the AdminCoreModuleFactory to create [your own admin module instead](./docs/admin-module.md).

3. Create a first AdminUser to log in with

```bash
yarn nestjs-admin createAdminUser # with yarn
npm run nestjs-admin createAdminUser # with npm
```

> By using the `DefaultAdminModule` you can create more AdminUsers directly from the administration interface!

> For now, the `DefaultAdminModule` is the only documented way to use nestjs-admin. If you need customisability, you'll have to dive into the source code.

4. Register entities in the admin site

```ts
// user.module.ts
import { TypeOrmModule } from '@nestjs/typeorm'
import { Module } from '@nestjs/common'
import { DefaultAdminModule, DefaultAdminSite } from 'nestjs-admin'
import { User } from './user.entity'

@Module({
  imports: [TypeOrmModule.forFeature([User]), DefaultAdminModule],
  exports: [TypeOrmModule],
})
export class UserModule {
  constructor(private readonly adminSite: DefaultAdminSite) {
    // Register the User entity under the "User" section
    adminSite.register('User', User)
  }
}
```

5. You can now login to access the admin interface at `/admin/login`!

Check the [rest of the docs](./docs) for more details.

## Contributing

Any contribution is welcome. If you want to implement a feature, you need to know that we are following [django-admin's API](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/) as closely as possible. Why?

- It's been well-thought-out by smart people
- Python translates well to Typescript
- It allows us to not have to think about what the API should look like and just use Django admin as a list of desirable features

### Start the example app

This repo is actually an example of how to use nestjs-admin. You can start it with:

```bash
yarn install
cp .env.example .env
docker-compose up
yarn migration:run
yarn start:debug
```

Then go to `localhost:8000/admin` to see an admin interface for a complex entity. The code for the actual library is in `libs/nestjs-admin`.

### Tooling

If you use VSCode, a `.vscode/` is committed that contains a good configuration to contribute. In particular, it contains a config for a ready-to-use debugger.

### ADRs

You'll find [ADRs](https://github.com/joelparkerhenderson/architecture_decision_record#suggestions-for-writing-good-adrs) in `docs/adr`
