# Support

Thanks for taking a look at Discount Deploy. It is commercetools-labs reference code: shared in the
hope that it is useful, and **unsupported**.

In practice that means:

- It is freely available under the MIT license ([LICENSE](LICENSE)) and is provided as-is, without
  warranty.
- It isn't covered by commercetools Support, your account team or any service-level agreement, so
  we can't promise replies, fixes or new releases.
- Issues and pull requests are welcome, and we'll take a look when we can.

## Before you point this at a production project

Deploying discounts into production is what this application is for, so these are worth knowing
before you do.

- **It writes to the target project.** Creating and updating cart discounts, product discounts,
  discount codes and discount groups is the whole function, so choose a target you are happy to
  have written to.
- **A least-privilege API client is the right fit**, rather than an admin one. The exact scope
  list is in the [README](README.md#the-target-projects-credentials). Nothing here needs access to
  products, orders or customers.
- **The diff comes first.** Every deploy is preceded by a preview showing `create`, `update` or
  `no change` per discount, and an update names the fields it would change. The preview and the
  deploy run the same comparison, so the preview is an accurate account of what the deploy will
  write.
- **Undoing a deploy is a manual step.** Nothing snapshots the target beforehand, so there is no
  rollback: a wrong deploy is undone by hand, or by deploying a corrected source discount over it.
- **A live discount takes effect immediately.** A discount deployed with `isActive: true` starts
  applying to carts as soon as it is written. To launch it later, deactivate it in the source
  first and deploy the activation separately.
- **Access is controlled by the Merchant Center.** Requests reach the API only through the
  Merchant Center API Gateway, which authenticates the user and confirms their access to the
  project; the function verifies the gateway's signature and requires the session's project to be
  the source project. So who may deploy is exactly who you granted the application to in the
  Merchant Center, and every request rides on that user's session rather than a shared API token.
  See [docs/setup.md](docs/setup.md#how-the-api-is-authenticated).
- **`npm run service` is for local development.** It is unauthenticated, binds to localhost and
  can write to the target project, so keep it on your own machine.

## If you need help

- **Questions and bugs:** open a GitHub issue. Another reader or a maintainer may well have an
  answer, though we can't commit to one.
- **Using it in production:** feel free to fork it and adapt it to your needs. Once it's in
  production it's yours, so review the code, test it against your projects and keep your copy up
  to date.
- **commercetools products:** for questions about Composable Commerce or the Merchant Center
  themselves, the [commercetools documentation](https://docs.commercetools.com), the community and
  commercetools Support are the right place, as usual.

The most useful bug report names the resource type, whether it was a create or an update, and
what the preview said compared with what happened. `npm run cli preview` reproduces most problems
without the Merchant Center in the way. Please keep real credentials, project keys you would
rather not publish, and customer data out of the issue.
