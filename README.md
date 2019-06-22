# ShellTest
Xamarin Shell

[![Build status](https://build.appcenter.ms/v0.1/apps/41054540-71e7-4cf3-ac58-407ef62f9673/branches/master/badge)](https://appcenter.ms)
[![Build status](https://build.appcenter.ms/v0.1/apps/b5d67465-a0da-41c5-a8cd-3cfec502ef1b/branches/master/badge)](https://appcenter.ms)

## Overview

Xamarin Shellが正式リリースされたので内容を確認する。

## Template

テンプレートに.NetCore Web APIプロジェクトを追加するチェックボックスがある。
単体試験用サーバーにそのまま使えそう。

UWPはまだShell対応していないため非活性になっている。

## Structure

コメントに記載されているFlyoutの実装内容。

```
    // These may be provided inline as below or as separate classes.

    // This header appears at the top of the Flyout.
    <Shell.FlyoutHeaderTemplate>
	<DataTemplate>
            <Grid>ContentHere</Grid>
        </DataTemplate>
    </Shell.FlyoutHeaderTemplate>

    // ItemTemplate is for ShellItems as displayed in a Flyout
    <Shell.ItemTemplate>
        <DataTemplate>
            <ContentView>
                Bindable Properties: Title, Icon
            </ContentView>
        </DataTemplate>
    </Shell.ItemTemplate>

    // MenuItemTemplate is for MenuItems as displayed in a Flyout
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <ContentView>
                Bindable Properties: Text, Icon
            </ContentView>
        </DataTemplate>
    </Shell.MenuItemTemplate>
```

コメントを解除してもハンバーガーメニューは出てこない。
これはテンプレートがTabBarをルートとしているため。

- ハンバーガーメニューを使用する場合
  - FlyoutItemをルートに置く
- ハンバーガーメニューを使用しない場合
  - TabBarをルートに置く

TabBarの代わりにFlyoutItemをルートとして、
その中にTabを記載すると二段構成になる。

- ハンバーガーをを使用してタブも使用する場合
  - FlyoutItemの中にTabを複数置く
- ハンバーガーをを使用してタブを使用しない場合
  - FlyoutItemの中にTabをひとつだけ置く

Flyoutのハンバーガーアイコンは戻るアイコンと同じ位置にある。
PushAsyncした状態だとハンバーガーを押せなくなる。
Googleアプリではどうなっていたか？？

ハンバーガーアイコンは別のアイコンに差し替え可能。

Flyoutをコードから開くことも可能。
こんなときはメニューを使えと提案するタイミングで表示できる。

### FlyoutHeaderTemplate

ハンバーガーメニューのヘッダー部を定義する。

iOSの場合はHeightRequestを指定しないと縦方向全部使おうとするので、
レイアウトを決めなければいけない。

### ItemTemplate

ItemとMenuItemがある。
ItemはTabと一致する内容に見えるがタブの有無とは無関係。
タブの有無はFlyout/TabBarの中にTab/ShellContentが複数あるかどうかで決まる。
ItemTemplateはハンバーガーメニューの外観を定義する。
FlyoutItemに設定されたプロパティを使用してデザインを作る。
このメニューを選択されるとFlyoutItem下のタブに遷移する。
どのタブが選択されているか覚えているので同時にインスタンス化されている。

### MenuItemTemplate

MenuItemは直接イベント処理を実行するための項目となる。
Shellのコードビハインド（またはバインド先）に処理を記述する。

ただしMenuItemの記載方法が現在のところ解明できていない。
暗黙的な変換演算子が機能していないことと関連するか？？
