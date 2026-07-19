## 作業ルール

- ユーザーからタスクMarkdownを指定された場合は、作業を始める前にそのファイルを最初に読む。
- 実装方針を決める前に、関連する既存コード、設定、コンポーネント、スタイルを確認する。
- 変更はタスクに記載された範囲に限定し、関係のないリファクタリングや整理を行わない。
- 既存のコンポーネントやスタイルを、可能な範囲で再利用する。
- 依存パッケージを追加する場合は、追加する理由と代替案の検討結果を説明する。
- UIやデザインを変更するときは `docs/design.md` に従う。既存UIと矛盾する場合は、既存の表現を尊重して調整する。
- 実装後は、`package.json` の scripts など、このリポジトリで利用可能なチェックとビルドを確認して実行する。現在定義されているビルドは `npm run build` である。

## 完了時の報告

最後に、以下を報告する。

- 変更概要
- 変更したファイル
- 実行した検証
- 残っている懸念点

## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
