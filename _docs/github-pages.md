---
title: GitHub Pages
permalink: /docs/github-pages/
---

[GitHub Pages](https://pages.github.com)は、ユーザー、組織、リポジトリの公開Webページで、`github.io`かカスタムドメイン名を選択でき、GitHubにホストされています。GitHub PagesはJekyllによって供給されているので、無料でJekyllで運営されているWebサイトをホストするのに最適な方法です。

<!-- [GitHub Pages](https://pages.github.com) are public web pages for users,
organizations, and repositories, that are freely hosted on GitHub's `github.io`
domain or on a custom domain name of your choice. GitHub Pages are powered by
Jekyll behind the scenes, so they're a great way to host your Jekyll-powered
website for free. -->

ソースファイルをプッシュすると、GitHub Pagesによりサイトが自動で生成されます。GitHub Pagesは通常のHTMLコンテンツでも同様に機能します。理由は簡単で、Jekyllはfront matterの無いファイルは静的なassetとして扱うためです。生成されたHTMLをプッシュするのでしたら、更なる設定を何もせずに行えばいいです。

<!-- Your site is automatically generated by GitHub Pages when you push your source
files. Note that GitHub Pages works equally well for regular HTML content,
simply because Jekyll treats files without front matter as static assets.
So if you only need to push generated HTML, you're good to go without any
further setup. -->

GitHub Pagesでサイトを構築するのは初めてですか？　[Jonathan McGloneによる素晴らしいガイド](http://jmcglone.com/guides/github-pages/){: target="_blank"}を見て、行ってみてください。このガイドでは、GitHub Pagesで自身のWebサイトを作るために、GitやGitHub、Jekyllについて知っておくべきことを説明しています。

<!-- Never built a website with GitHub Pages before? [See this marvelous guide by
Jonathan McGlone](http://jmcglone.com/guides/github-pages/) to get you up and
running. This guide will teach you what you need to know about Git, GitHub, and
Jekyll to create your very own website on GitHub Pages. -->

##  The github-pages gem

GitHubの友人たちは、[JekyllとそのGitHub Pagesへの依存関係](https://pages.github.com/versions/){: target="_blenk"}を管理するために使用される[github-pages](https://github.com/github/pages-gem)を提供しています。プロジェクトでそれを使うことは、サイトをGitHub Pagesにデプロイするとき、gemの様々なバージョン間の予想外の違いに捕まることはないということを意味します。

<!-- Our friends at GitHub have provided the
[github-pages](https://github.com/github/pages-gem) gem which is used to manage
[Jekyll and its dependencies on GitHub Pages](https://pages.github.com/versions/).
Using it in your projects means that when you deploy your site to GitHub Pages,
you will not be caught by unexpected differences between various versions of the
gems. -->

GitHub Pagesは`safe`モードで動作し、[ホワイトリストに登録されたプラグインのセット](https://help.github.com/articles/configuring-jekyll-plugins/#default-plugins){: target="_blenk"}のみを許可していることに気をつけてください。

<!-- Note that GitHub Pages runs in `safe` mode and only allows [a set of whitelisted
plugins](https://help.github.com/articles/configuring-jekyll-plugins/#default-plugins). -->

プロジェクトで現在デプロイされているバージョンのgemを使用するには、`Gemfile`に以下を追加します。

<!-- To use the currently-deployed version of the gem in your project, add the
following to your `Gemfile`: -->

```ruby
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
```

必ず`bundle update`を頻繁に実行してください。

<!-- Be sure to run `bundle update` often. -->

<div class="note">
  <h5>GitHub Pagesのドキュメンテーションとヘルプ、サポート</h5>
  <!-- <h5>GitHub Pages Documentation, Help, and Support</h5> -->
  <p>
    GitHub Pagesでできることやトラブルシューティングガイドの詳細については、<a href="https://help.github.com/categories/github-pages-basics/" target="_blenk">GitHubのPagesヘルプセクション</a>をご覧ください。 それでもうまくいかない場合は、<a href="https://github.com/contact" target="_blenk">GitHubサポート</a>に連絡してください。
  </p>
  <!-- <p>
    For more information about what you can do with GitHub Pages, as well as for
    troubleshooting guides, you should check out
    <a href="https://help.github.com/categories/github-pages-basics/">GitHub’s Pages Help section</a>.
    If all else fails, you should contact <a href="https://github.com/contact">GitHub Support</a>.
  </p> -->
</div>

### プロジェクトページのURL構成
<!-- ### Project Page URL Structure -->

GitHubにプッシュする前にJekyllサイトをプレビューした方が良い場合があります。GitHubがプロジェクトページに使用するサブディレクトリのようなURL構造は、URLの適切な解決を複雑にします。あなたのサイトが正しく構築されていることを確認するために、便利な[URLフィルタ]({{ "/docs/liquid/filters/" | relative_url }})を使用してください。

<!-- Sometimes it's nice to preview your Jekyll site before you push your `gh-pages`
branch to GitHub. The subdirectory-like URL structure GitHub uses for
Project Pages complicates the proper resolution of URLs. In order to assure your
site builds properly, use the handy [URL filters](/docs/liquid/filters/): -->

{% raw %}
```liquid
<!-- For styles with static names... -->
<link href="{{ "/assets/css/style.css" | relative_url }}" rel="stylesheet">
<!-- For documents/pages whose URLs can change... -->
[{{ page.title }}]("{{ page.url | relative_url }}")
```
{% endraw %}

この方法で、localhost上のサイトrootからローカルでプレビューできます。そして、GitHubがページを生成するときには、全てのURLが正しく解決されます。

<!-- This way you can preview your site locally from the site root on localhost,
but when GitHub generates your pages from the `gh-pages` branch all the URLs
will resolve properly. -->

## JekyllをGitHub Pagesにデプロイ
<!-- ## Deploying Jekyll to GitHub Pages -->

GitHub PagesはGitHub上のリポジトリの特定のブランチを見ることによって機能します。利用可能な2つの基本的な種類があります：[user/organization と project pages](https://help.github.com/articles/user-organization-and-project-pages/){: target="_blenk"}。これら2種類のサイトを展開する方法は、いくつかの細かい点を除いてほぼ同じです。

<!-- GitHub Pages work by looking at certain branches of repositories on GitHub.
There are two basic types available: [user/organization and project pages](https://help.github.com/articles/user-organization-and-project-pages/).
The way to deploy these two types of sites are nearly identical, except for a
few minor details. -->

### UserとOrganizationページ
<!-- ### User and Organization Pages -->

UserとOrganizationページは、GitHub Pagesのファイル専用の特別なGitHubリポジトリにあります。このリポジトリは、アカウント名にちなんで命名する必要があります。たとえば、[@mojomboのユーザーページのリポジトリ](https://github.com/mojombo/mojombo.github.io){: target="_blenk"}の名前は`mojombo.github.io`です。

<!-- User and organization pages live in a special GitHub repository dedicated to
only the GitHub Pages files. This repository must be named after the account
name. For example, [@mojombo’s user page repository](https://github.com/mojombo/mojombo.github.io) has the name
`mojombo.github.io`. -->

GitHub Pagesサイトの構築と公開には、リポジトリの`master`ブランチのコンテンツが使用されます。そのため、Jekyllサイトがそこに格納されていることを確認してください。

<!-- Content from the `master` branch of your repository will be used to build and
publish the GitHub Pages site, so make sure your Jekyll site is stored there. -->

<div class="note info">
  <h5>カスタムドメインはリポジトリ名に影響しません</h5>
  <!-- <h5>Custom domains do not affect repository names</h5> -->
  <p>
    GitHub Pagesは、まず<code>username.github.io</code>サブドメインの下に存在するように設定されています。そのため、<strong>カスタムドメインを使用している場合でも</strong>、リポジトリにこのように名前を付ける必要があります。
  </p>
  <!-- <p>
    GitHub Pages are initially configured to live under the
    <code>username.github.io</code> subdomain, which is why repositories must
    be named this way <strong>even if a custom domain is being used</strong>.
  </p> -->
</div>

### Project Pages

UserとOrganizationページとは異なり、プロジェクトページは、目的のプロジェクトと同じリポジトリに保存されます。ただし、Webページのコンテンツは、特別な名前の`gh-pages`ブランチ、または`master`ブランチの`docs`フォルダに格納されているます。コンテンツはJekyllを使用してレンダリングされ、出力は`username.github.io/project`などのユーザーページサブドメインのサブパスの下に表示されます（カスタムドメインが指定されていない場合）。

<!-- Unlike user and organization Pages, Project Pages are kept in the same
repository as the project they are for, except that the website content is
stored in a specially named `gh-pages` branch or in a `docs` folder on the
`master` branch. The content will be rendered using Jekyll, and the output
will become available under a subpath of your user pages subdomain, such as
`username.github.io/project` (unless a custom domain is specified). -->

Jekyllプロジェクトのリポジトリ自体が、このブランチ構造の完璧な例です。[マスターブランチ]({{ site.repository }}){: target="_blenk"} にはJekyllの実際のソフトウェアプロジェクトが含まれています。現在見ているJekyll Webサイトは、同じリポジトリの[docsフォルダ]({{ site.repository }}/tree/master/docs){: target="_blenk"}にあります。

<!-- The Jekyll project repository itself is a perfect example of this branch
structure—the [master branch]({{ site.repository }}) contains the
actual software project for Jekyll, and the Jekyll website that you’re
looking at right now is contained in the [docs
folder]({{ site.repository }}/tree/master/docs) of the same repository. -->

より詳細な例については、GitHubの公式ドキュメントの[user, organization and project pages](https://help.github.com/articles/user-organization-and-project-pages/){: target="_blenk"}を参照してください。

<!-- Please refer to GitHub official documentation on
[user, organization and project pages](https://help.github.com/articles/user-organization-and-project-pages/)
to see more detailed examples. -->

<div class="note warning">
  <h5>ソースファイルはrootディレクトリに必要です</h5>
  <!-- <h5>Source files must be in the root directory</h5> -->
  <p>
GitHub Pagesは<a href="{{ "/docs/configuration/" | relative_url }}">“Site Source”</a>の設定値を<a href="https://help.github.com/articles/troubleshooting-github-pages-build-failures#source-setting" target="_blank">上書き</a>するため、ファイルをルートディレクトリ以外の場所に配置すると、サイトが正しく構築されない可能性があります。  </p>
  <!-- <p>
    GitHub Pages <a href="https://help.github.com/articles/troubleshooting-github-pages-build-failures#source-setting">overrides</a>
    the <a href="/docs/configuration/options/>“Site Source”</a>
    configuration value, so if you locate your files anywhere other than the
    root directory, your site may not build correctly.
  </p> -->
</div>

<div class="note info">
  <h5>Windowsでの<code>github-pages</code> gemのインストール</h5>
  <!-- <h5>Installing the <code>github-pages</code> gem on Windows</h5> -->

  <p>
    Windowsは公式にサポートされていませんが、Windows上に<code>github-pages</code> gemをインストールすることは可能です。詳細は<a href="{{ "/docs/installation/windows/" | relative_url }}">Windowsの特別なページ</a>をご覧ください。
  </p>
  <!-- <p>
    While Windows is not officially supported, it is possible
    to install the <code>github-pages</code> gem on Windows.
    Special instructions can be found on our
    <a href="/docs/installation/windows/">Windows-specific docs page</a>.
  </p> -->
</div>
