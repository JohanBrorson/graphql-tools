# @graphql-tools/federation

## 1.1.28

### Patch Changes

- [#6091](https://github.com/ardatan/graphql-tools/pull/6091) [`9bca9e0`](https://github.com/ardatan/graphql-tools/commit/9bca9e03915a2e12d164e355be9aed389b0de3a4) Thanks [@User](https://github.com/User), [@User](https://github.com/User)! - If the gateway receives a query with an overlapping fields for the subschema, it uses aliases to resolve it correctly.

  Let's say subschema A has the following schema;

  ```graphql
    type Query {

    }

    interface User {
      id: ID!
      name: String!
    }

    type Admin implements User {
      id: ID!
      name: String!
      role: String!
    }

    type Customer implements User {
      id: ID!
      name: String
      email: String
    }
  ```

  And let's say the gateway has the following schema instead;

  ```graphql
    type Query {

    }

    interface User {
      id: ID!
      name: String!
    }

    type Admin implements User {
      id: ID!
      name: String!
      role: String!
    }

    type Customer implements User {
      id: ID!
      name: String!
      email: String!
    }
  ```

  In this case, the following query is fine for the gateway but for the subschema, it's not;

  ```graphql
  query {
    user {
      ... on Admin {
        id
        name # This is nullable in the subschema
        role
      }
      ... on Customer {
        id
        name # This is non-nullable in the subschema
        email
      }
    }
  }
  ```

  So the subgraph will throw based on this rule [OverlappingFieldsCanBeMerged](https://github.com/graphql/graphql-js/blob/main/src/validation/rules/OverlappingFieldsCanBeMergedRule.ts)

  To avoid this, the gateway will use aliases to resolve the query correctly. The query will be transformed to the following;

  ```graphql
  query {
    user {
      ... on Admin {
        id
        name # This is nullable in the subschema
        role
      }
      ... on Customer {
        id
        name: _nullable_name # This is non-nullable in the subschema
        email
      }
    }
  }
  ```

- Updated dependencies [[`9bca9e0`](https://github.com/ardatan/graphql-tools/commit/9bca9e03915a2e12d164e355be9aed389b0de3a4), [`9bca9e0`](https://github.com/ardatan/graphql-tools/commit/9bca9e03915a2e12d164e355be9aed389b0de3a4), [`243c353`](https://github.com/ardatan/graphql-tools/commit/243c353412921cf0063f963ee46b9c63d2f33b41)]:
  - @graphql-tools/stitch@9.2.0
  - @graphql-tools/delegate@10.0.5

## 1.1.27

### Patch Changes

- [#6086](https://github.com/ardatan/graphql-tools/pull/6086) [`f538e50`](https://github.com/ardatan/graphql-tools/commit/f538e503c3cdb152bd29f77804217100cac0f648) Thanks [@ardatan](https://github.com/ardatan)! - Handle @inaccessible types correctly

## 1.1.26

### Patch Changes

- [#6071](https://github.com/ardatan/graphql-tools/pull/6071) [`6cf507f`](https://github.com/ardatan/graphql-tools/commit/6cf507fc70d2474c71c8604ab117d01af76376e1) Thanks [@ardatan](https://github.com/ardatan)! - Handle inaccessible enum values

## 1.1.25

### Patch Changes

- [`e09c383`](https://github.com/ardatan/graphql-tools/commit/e09c383a540f84f56db141466b711f88fce8548d) Thanks [@ardatan](https://github.com/ardatan)! - Respect fields with specified types

## 1.1.24

### Patch Changes

- [`458ef46`](https://github.com/ardatan/graphql-tools/commit/458ef46536db003edc399587feabfcee7b186830) Thanks [@ardatan](https://github.com/ardatan)! - Remove extra logs

## 1.1.23

### Patch Changes

- [`2202768`](https://github.com/ardatan/graphql-tools/commit/220276800d271e7c6fbc43339eb779b618c82e68) Thanks [@ardatan](https://github.com/ardatan)! - Federation v1 support improvements

## 1.1.22

### Patch Changes

- [`4620bb2`](https://github.com/ardatan/graphql-tools/commit/4620bb2a352fd0e645950aaae8bb54cbc7c85ce7) Thanks [@ardatan](https://github.com/ardatan)! - Handle unspecified key fields

## 1.1.21

### Patch Changes

- [`14f4fae`](https://github.com/ardatan/graphql-tools/commit/14f4faec87b1423c5541dab16dc2c5c1298edcf7) Thanks [@ardatan](https://github.com/ardatan)! - Handle orphan scalars with directives

## 1.1.20

### Patch Changes

- [`b78ce7e`](https://github.com/ardatan/graphql-tools/commit/b78ce7e42c8d016d972b125a86508f5ab78d57a6) Thanks [@ardatan](https://github.com/ardatan)! - Handle orphan union types

## 1.1.19

### Patch Changes

- [#5956](https://github.com/ardatan/graphql-tools/pull/5956) [`d4395dd`](https://github.com/ardatan/graphql-tools/commit/d4395dd7d21db3becdf51cc0508e35d246dcbe1e) Thanks [@ardatan](https://github.com/ardatan)! - Handle orphan types

- Updated dependencies [[`8199416`](https://github.com/ardatan/graphql-tools/commit/81994160488aad1114b0d130083bcf694fe13aba), [`baf3c28`](https://github.com/ardatan/graphql-tools/commit/baf3c28f43dcfafffd15386daeb153bc2895c1b3)]:
  - @graphql-tools/wrap@10.0.3
  - @graphql-tools/utils@10.1.1

## 1.1.18

### Patch Changes

- [#5946](https://github.com/ardatan/graphql-tools/pull/5946) [`107c021`](https://github.com/ardatan/graphql-tools/commit/107c021aa191f0654c45ed72b45d650993e2142f) Thanks [@ardatan](https://github.com/ardatan)! - If an interface or scalar type is not annotated for a subgraph explicitly, consider them as a shared type

## 1.1.17

### Patch Changes

- [#5913](https://github.com/ardatan/graphql-tools/pull/5913) [`83c0af0`](https://github.com/ardatan/graphql-tools/commit/83c0af0713ff2ce55ccfb97a1810ecfecfeab703) Thanks [@enisdenjo](https://github.com/enisdenjo)! - dependencies updates:
  - Updated dependency [`@graphql-tools/delegate@^10.0.3` ↗︎](https://www.npmjs.com/package/@graphql-tools/delegate/v/10.0.3) (from `^10.0.1`, in `dependencies`)
  - Updated dependency [`@graphql-tools/executor-http@^1.0.8` ↗︎](https://www.npmjs.com/package/@graphql-tools/executor-http/v/1.0.8) (from `^1.0.6`, in `dependencies`)
  - Updated dependency [`@graphql-tools/merge@^9.0.1` ↗︎](https://www.npmjs.com/package/@graphql-tools/merge/v/9.0.1) (from `^9.0.0`, in `dependencies`)
  - Updated dependency [`@graphql-tools/schema@^10.0.2` ↗︎](https://www.npmjs.com/package/@graphql-tools/schema/v/10.0.2) (from `^10.0.0`, in `dependencies`)
  - Updated dependency [`@graphql-tools/stitch@^9.0.4` ↗︎](https://www.npmjs.com/package/@graphql-tools/stitch/v/9.0.4) (from `^9.0.2`, in `dependencies`)
  - Updated dependency [`@graphql-tools/utils@^10.0.13` ↗︎](https://www.npmjs.com/package/@graphql-tools/utils/v/10.0.13) (from `^10.0.0`, in `dependencies`)
  - Updated dependency [`@graphql-tools/wrap@^10.0.1` ↗︎](https://www.npmjs.com/package/@graphql-tools/wrap/v/10.0.1) (from `^10.0.0`, in `dependencies`)
- Updated dependencies [[`83c0af0`](https://github.com/ardatan/graphql-tools/commit/83c0af0713ff2ce55ccfb97a1810ecfecfeab703), [`83c0af0`](https://github.com/ardatan/graphql-tools/commit/83c0af0713ff2ce55ccfb97a1810ecfecfeab703), [`83c0af0`](https://github.com/ardatan/graphql-tools/commit/83c0af0713ff2ce55ccfb97a1810ecfecfeab703), [`83c0af0`](https://github.com/ardatan/graphql-tools/commit/83c0af0713ff2ce55ccfb97a1810ecfecfeab703), [`83c0af0`](https://github.com/ardatan/graphql-tools/commit/83c0af0713ff2ce55ccfb97a1810ecfecfeab703), [`83c0af0`](https://github.com/ardatan/graphql-tools/commit/83c0af0713ff2ce55ccfb97a1810ecfecfeab703), [`83c0af0`](https://github.com/ardatan/graphql-tools/commit/83c0af0713ff2ce55ccfb97a1810ecfecfeab703)]:
  - @graphql-tools/delegate@10.0.4
  - @graphql-tools/executor-http@1.0.9
  - @graphql-tools/merge@9.0.3
  - @graphql-tools/schema@10.0.3
  - @graphql-tools/stitch@9.0.5
  - @graphql-tools/wrap@10.0.2

## 1.1.16

### Patch Changes

- [`7583729`](https://github.com/ardatan/graphql-tools/commit/7583729718ffd528bba5d1c5c4ea087975102c1f) Thanks [@ardatan](https://github.com/ardatan)! - Fix `getSubschemaForFederationWithTypeDefs` for non-supergraph merging of subgraphs

## 1.1.15

### Patch Changes

- [#5885](https://github.com/ardatan/graphql-tools/pull/5885) [`2d76909`](https://github.com/ardatan/graphql-tools/commit/2d76909908a918562a9f7599825b70ae60f91127) Thanks [@ardatan](https://github.com/ardatan)! - Avoid creating invalid schema when there is no entity

## 1.1.14

### Patch Changes

- [#5878](https://github.com/ardatan/graphql-tools/pull/5878) [`ba062ff`](https://github.com/ardatan/graphql-tools/commit/ba062ff4880f6922eaddfcbd746782275a8f689e) Thanks [@darren-west](https://github.com/darren-west)! - fix: buildSubgraphSchema with no entity keys

## 1.1.13

### Patch Changes

- [`974df8a`](https://github.com/ardatan/graphql-tools/commit/974df8a1a1bca422bac5d971a3f8029cd9728efd) Thanks [@ardatan](https://github.com/ardatan)! - Debug logging & expose the subgraph schema

- Updated dependencies [[`b798b3b`](https://github.com/ardatan/graphql-tools/commit/b798b3b0a54f634bf2dd2275ef47f5263a5ce238)]:
  - @graphql-tools/executor-http@1.0.6

## 1.1.12

### Patch Changes

- [`efedc590`](https://github.com/ardatan/graphql-tools/commit/efedc59018ea1d63f86973d0c6608b3c7ddc2e71)
  Thanks [@ardatan](https://github.com/ardatan)! - Handle unions correctly

## 1.1.11

### Patch Changes

- [`250715a1`](https://github.com/ardatan/graphql-tools/commit/250715a1e18f0c645240ea78bb80f7557ac81340)
  Thanks [@ardatan](https://github.com/ardatan)! - Support `extend type` in subgraph SDL

- [`250715a1`](https://github.com/ardatan/graphql-tools/commit/250715a1e18f0c645240ea78bb80f7557ac81340)
  Thanks [@ardatan](https://github.com/ardatan)! - Support supergraph with no join\_\_type
  directives on Query type

## 1.1.10

### Patch Changes

- [`cda328c3`](https://github.com/ardatan/graphql-tools/commit/cda328c3e487ea51e13a3b18f0e2e494fd3275ca)
  Thanks [@ardatan](https://github.com/ardatan)! - Support for multiple key entrypoints for an
  object, and avoid sending whole object if possible

- Updated dependencies
  [[`cda328c3`](https://github.com/ardatan/graphql-tools/commit/cda328c3e487ea51e13a3b18f0e2e494fd3275ca)]:
  - @graphql-tools/stitch@9.0.2

## 1.1.9

### Patch Changes

- [`3ed8cbd6`](https://github.com/ardatan/graphql-tools/commit/3ed8cbd68988492e8b220a82b3590bad2a1c672b)
  Thanks [@ardatan](https://github.com/ardatan)! - Support @join\_\_implements in Federation

## 1.1.8

### Patch Changes

- [`7fe63895`](https://github.com/ardatan/graphql-tools/commit/7fe63895c1b989de3ab433e90945cb318718ddac)
  Thanks [@ardatan](https://github.com/ardatan)! - Fix Fed v2 support

## 1.1.7

### Patch Changes

- [#5579](https://github.com/ardatan/graphql-tools/pull/5579)
  [`d30e8735`](https://github.com/ardatan/graphql-tools/commit/d30e8735682c3a7209cded3fc16dd889ddfa5ddf)
  Thanks [@ardatan](https://github.com/ardatan)! - Optimizations and refactor

## 1.1.6

### Patch Changes

- [`9b404e83`](https://github.com/ardatan/graphql-tools/commit/9b404e8346af2831e3ed56326cd9e1e9f8582b42)
  Thanks [@ardatan](https://github.com/ardatan)! - Handle type ownerships correctly

## 1.1.5

### Patch Changes

- [#5567](https://github.com/ardatan/graphql-tools/pull/5567)
  [`61393975`](https://github.com/ardatan/graphql-tools/commit/61393975c535e45c108500feea1ceec461586c6e)
  Thanks [@ardatan](https://github.com/ardatan)! - Respect input types

## 1.1.4

### Patch Changes

- [#5559](https://github.com/ardatan/graphql-tools/pull/5559)
  [`ada5c56a`](https://github.com/ardatan/graphql-tools/commit/ada5c56af472e06d595e53a035c105e745490bfc)
  Thanks [@ardatan](https://github.com/ardatan)! - Support unowned types such as interfaces, unions
  and scalars

## 1.1.3

### Patch Changes

- [#5474](https://github.com/ardatan/graphql-tools/pull/5474)
  [`f31be313`](https://github.com/ardatan/graphql-tools/commit/f31be313b2af5a7c5bf893f1ce1dc7d36bf5340c)
  Thanks [@ardatan](https://github.com/ardatan)! - dependencies updates:

  - Removed dependency [`lodash.pick@^4.4.0` ↗︎](https://www.npmjs.com/package/lodash.pick/v/4.4.0)
    (from `dependencies`)

- [#5474](https://github.com/ardatan/graphql-tools/pull/5474)
  [`f31be313`](https://github.com/ardatan/graphql-tools/commit/f31be313b2af5a7c5bf893f1ce1dc7d36bf5340c)
  Thanks [@ardatan](https://github.com/ardatan)! - Optimizations for federation

- Updated dependencies
  [[`f31be313`](https://github.com/ardatan/graphql-tools/commit/f31be313b2af5a7c5bf893f1ce1dc7d36bf5340c)]:
  - @graphql-tools/delegate@10.0.1
  - @graphql-tools/stitch@9.0.1

## 1.1.2

### Patch Changes

- [#5468](https://github.com/ardatan/graphql-tools/pull/5468)
  [`de9e8a67`](https://github.com/ardatan/graphql-tools/commit/de9e8a678a0ab38e5fc1cbf6c1bf27c265cc0c01)
  Thanks [@ardatan](https://github.com/ardatan)! - dependencies updates:

  - Added dependency [`lodash.pick@^4.4.0` ↗︎](https://www.npmjs.com/package/lodash.pick/v/4.4.0)
    (to `dependencies`)

- [#5468](https://github.com/ardatan/graphql-tools/pull/5468)
  [`de9e8a67`](https://github.com/ardatan/graphql-tools/commit/de9e8a678a0ab38e5fc1cbf6c1bf27c265cc0c01)
  Thanks [@ardatan](https://github.com/ardatan)! - Reduce the number of upstream requests

## 1.1.1

### Patch Changes

- [`d593dfce`](https://github.com/ardatan/graphql-tools/commit/d593dfce52a895993c754903687043a9d5429803)
  Thanks [@ardatan](https://github.com/ardatan)! - Adding `batch` option to allow batching

## 1.1.0

### Minor Changes

- [#5455](https://github.com/ardatan/graphql-tools/pull/5455)
  [`d4de4a8e`](https://github.com/ardatan/graphql-tools/commit/d4de4a8e84f7dabbaab058b264a350a3592dd752)
  Thanks [@ardatan](https://github.com/ardatan)! - Supergraph SDL support

## 1.0.0

### Major Changes

- [#5274](https://github.com/ardatan/graphql-tools/pull/5274)
  [`944a68e8`](https://github.com/ardatan/graphql-tools/commit/944a68e8becf9c86b4c97fd17c372d98a285b955)
  Thanks [@ardatan](https://github.com/ardatan)! - Drop Node 14 support. Require Node.js `>= 16`

### Patch Changes

- Updated dependencies
  [[`944a68e8`](https://github.com/ardatan/graphql-tools/commit/944a68e8becf9c86b4c97fd17c372d98a285b955),
  [`8fba6cc1`](https://github.com/ardatan/graphql-tools/commit/8fba6cc1876e914d587f5b253332aaedbcaa65e6),
  [`944a68e8`](https://github.com/ardatan/graphql-tools/commit/944a68e8becf9c86b4c97fd17c372d98a285b955),
  [`944a68e8`](https://github.com/ardatan/graphql-tools/commit/944a68e8becf9c86b4c97fd17c372d98a285b955),
  [`944a68e8`](https://github.com/ardatan/graphql-tools/commit/944a68e8becf9c86b4c97fd17c372d98a285b955)]:
  - @graphql-tools/executor-http@1.0.0
  - @graphql-tools/delegate@10.0.0
  - @graphql-tools/schema@10.0.0
  - @graphql-tools/stitch@9.0.0
  - @graphql-tools/merge@9.0.0
  - @graphql-tools/utils@10.0.0
  - @graphql-tools/wrap@10.0.0

## 0.0.3

### Patch Changes

- [#5223](https://github.com/ardatan/graphql-tools/pull/5223)
  [`24c13616`](https://github.com/ardatan/graphql-tools/commit/24c136160fe675c08c1c1fe06bfb8883cdf0b466)
  Thanks [@ardatan](https://github.com/ardatan)! - dependencies updates:
  - Updated dependency
    [`@graphql-tools/executor-http@^0.1.9` ↗︎](https://www.npmjs.com/package/@graphql-tools/executor-http/v/0.1.9)
    (from `^0.0.7`, in `dependencies`)

## 0.0.2

### Patch Changes

- [#5212](https://github.com/ardatan/graphql-tools/pull/5212)
  [`0cd9e8c4`](https://github.com/ardatan/graphql-tools/commit/0cd9e8c4469d07e53ad8e7944ba144f58c4db34f)
  Thanks [@ardatan](https://github.com/ardatan)! - dependencies updates:

  - Updated dependency
    [`@graphql-tools/delegate@^9.0.19` ↗︎](https://www.npmjs.com/package/@graphql-tools/delegate/v/9.0.19)
    (from `9.0.19`, in `dependencies`)
  - Updated dependency
    [`@graphql-tools/merge@^8.3.16` ↗︎](https://www.npmjs.com/package/@graphql-tools/merge/v/8.3.16)
    (from `8.3.16`, in `dependencies`)
  - Updated dependency
    [`@graphql-tools/schema@^9.0.14` ↗︎](https://www.npmjs.com/package/@graphql-tools/schema/v/9.0.14)
    (from `9.0.14`, in `dependencies`)
  - Updated dependency
    [`@graphql-tools/wrap@^9.2.20` ↗︎](https://www.npmjs.com/package/@graphql-tools/wrap/v/9.2.20)
    (from `9.2.20`, in `dependencies`)
  - Updated dependency
    [`@graphql-tools/utils@^9.1.3` ↗︎](https://www.npmjs.com/package/@graphql-tools/utils/v/9.1.3)
    (from `9.1.3`, in `dependencies`)
  - Updated dependency
    [`@graphql-tools/executor-http@^0.0.7` ↗︎](https://www.npmjs.com/package/@graphql-tools/executor-http/v/0.0.7)
    (from `0.0.7`, in `dependencies`)
  - Updated dependency
    [`@graphql-tools/stitch@^8.7.34` ↗︎](https://www.npmjs.com/package/@graphql-tools/stitch/v/8.7.34)
    (from `8.7.34`, in `dependencies`)
  - Added dependency
    [`value-or-promise@^1.0.12` ↗︎](https://www.npmjs.com/package/value-or-promise/v/1.0.12) (to
    `dependencies`)

- [#5215](https://github.com/ardatan/graphql-tools/pull/5215)
  [`88244048`](https://github.com/ardatan/graphql-tools/commit/882440487551abcb5bdd4f626f3b56ac2e886f11)
  Thanks [@ardatan](https://github.com/ardatan)! - Avoid object spread

- [#5220](https://github.com/ardatan/graphql-tools/pull/5220)
  [`8e80b689`](https://github.com/ardatan/graphql-tools/commit/8e80b6893d2342353731610d5da9db633d806083)
  Thanks [@ardatan](https://github.com/ardatan)! - Performance improvements

- Updated dependencies
  [[`8e80b689`](https://github.com/ardatan/graphql-tools/commit/8e80b6893d2342353731610d5da9db633d806083)]:
  - @graphql-tools/delegate@9.0.35
  - @graphql-tools/stitch@8.7.49

## 0.0.1

### Patch Changes

- [#4974](https://github.com/ardatan/graphql-tools/pull/4974)
  [`1c0e80a6`](https://github.com/ardatan/graphql-tools/commit/1c0e80a60827169eb3eb99fe5710b1e891b89740)
  Thanks [@ardatan](https://github.com/ardatan)! - New Federation package
