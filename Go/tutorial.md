app

```go
var Deligate func(ctx context.Context, f func(event.Repository) error)

// アーティスアカウントが作成された時のドメインイベントを処理します
//
// オブジェクトを`*`でポリシーを登録します。
var HandleArtisAccountCreatedEvent = func(ctx context.Context, m map[string]interface{}) error {
	Deligate(ctx, func(repo event.Repository) error {
		aid := seeker.Str(m, []string{"account_id"})

		// TODO: ポリシーを登録する処理は関数にして共通化
		s, err := authz.NewSubject(aid)
		if err != nil {
			return errors.NewError("サブジェクトを作成できません", err)
		}

		o, err := authz.NewObject(authz.AdminObject)
		if err != nil {
			return errors.NewError("オブジェクトを作成できません", err)
		}

		az, err := authz.NewAuthz(s, o)
		if err != nil {
			return errors.NewError("認可を作成できません", err)
		}

		if err := repo.Store(az); err != nil {
			return errors.NewError("認可を登録できません", err)
		}

		return nil
	})
	//proc, err := di.InitEventProc(ctx)
	//if err != nil {
	//	return errors.NewError("イベント処理用のサブスクライバを初期化できません", err)
	//}

	return nil
}
```
gateway

```go
func init() {
	account.Deligate = deligate
}

var deligate = func(ctx context.Context, f func(repository event.Repository) error) {
	repo, _ := NewRepository(ctx)

	f(repo)
}
```
