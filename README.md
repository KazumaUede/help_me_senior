# README

help_me_senior とは
先輩への質問をスタミナ制にし、適切な質問回数を視覚的に表現するチャットアプリ
https://qiita.com/y-tsuna/items/fced3a9515c8f585ca50
このブログを見て面白そうでかつ自分としても有用かと思い、relationshipテーブルの練習も兼ねて作ることにしました。

目標の機能
1.ユーザーサインアップ時に先輩か後輩かを宣言します。
2.先輩は後輩をグループに招待することができます。(先輩後輩問わず複数可能)
3.後輩はスタミナを最大４つ持ち、一つ消費することで先輩に質問することができます。
4.先輩は基本的には質問されたら答えていただきます。簡単な質問であれば本当に行使していいのかを伝えて断るという駆け引きもありとします。その際はスタミナは元に戻ります。
5.スタミナは午前5時に１回復します。



## usersテーブル

|Columns      |Type     |Options                 |
|-------------|---------|------------------------|
|nickname     |string   |null: false             |
|email        |string   |null: false             |
|password     |string   |null: false             |
|player       |integer  |null: false             |
|hearts       |integer  |null: false, default: 30|

### Association

- has_many :passive_relationships,class_name:  "Relationship", foreign_key: "following_id", dependent: :destroy
- has_many :followedes, through: :passive_relationships
- has_many :active_relationships,class_name:  "Relationship", foreign_key: "followed_id", dependent: :destroy
- has_many :followings, through: :active_relationships

- has_many :group_users
- has_many :groups, through: :group_users
- has_many :masseges

## relationshipsテーブル

|Column      |Type      |Options     |
|------------|----------|------------|
|followed_id |integer   |null: false |
|following_id|integer   |null: false |

### Association
- belongs_to :following, class_name: "User", foreign_key: "following_id"
- belongs_to :followed, class_name: "User", foreign_key: "followed_id"


## groupsテーブル

|Column  |Type       |Options                       |
|--------|-----------|------------------------------|
|group_id|integer    |null: false                   |

### Association
- has_many :users, through: :group_users
- has_many :group_users
- accepts_nested_attributes_for :group_users, allow_destroy: true     <!-- group側からuser側を配列として触れるようにするメソッド -->
- has_many :messages



## group_usersテーブル

|Column  |Type      |Options                         |
|--------|----------|--------------------------------|
|user_id |references|index: true, foreign_key: true  |
|group_id|references|index: true, fore 5ign_key: true|

### Association
- belongs_to :group
- belongs_to :user



## messagesテーブル

|Column  |Type      |Options                       |
|--------|----------|------------------------------|
|text    |string    |                              |
|image   |string    |                              |
|user_id |references|null: false, foreign_key: true|
|group_id|references|null: false, foreign_key: true|

### Association
- belongs_to :group
- belongs_to :user


