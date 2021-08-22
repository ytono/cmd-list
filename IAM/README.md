# IAM

## Custom Role
```
# リソースで使用可能な権限を表示
gcloud iam list-testable-permissions //cloudresourcemanager.googleapis.com/projects/$PROJECT_ID

# 役割メタデータを取得
gcloud iam roles describe [ROLE_NAME]

# リソースに対して付与できる役割を表示
gcloud iam list-grantable-roles //cloudresourcemanager.googleapis.com/projects/$PROJECT_ID
```

```
# カスタムの役割を作成
gcloud iam roles create editor --project $PROJECT_ID --file role-definition.yaml

    title: [ROLE_TITLE]
    description: [ROLE_DESCRIPTION]
    stage: [LAUNCH_STAGE]
    includedPermissions:
    - [PERMISSION_1]
    - [PERMISSION_2]

    # role-definition.yaml
    title: "Role Editor"
    description: "Edit access for App Versions"
    stage: "ALPHA"
    includedPermissions:
    - appengine.versions.create
    - appengine.versions.delete


gcloud iam roles create viewer --project $PROJECT_ID \
--title "Role Viewer" \
--description "Custom role description." \
--permissions compute.instances.get,compute.instances.list \
--stage ALPHA
```

```
# カスタムの役割を一覧表示
gcloud iam roles list --project $PROJECT_ID

# 削除された役割を一覧表示
gcloud iam roles list --show-deleted --project $PROJECT_ID
```

```
# 既存のカスタムの役割を編集
gcloud iam roles describe [ROLE_ID] --project $PROJECT_ID > new-role-definition.yaml

gcloud iam roles update [ROLE_ID] --project $PROJECT_ID \
--file new-role-definition.yaml 

gcloud iam roles update viewer --project $PROJECT_ID \
--add-permissions storage.buckets.get,storage.buckets.list

```

```
# カスタムの役割を無効化
gcloud iam roles update viewer --project $PROJECT_ID --stage DISABLED

# カスタムの役割を削除  
gcloud iam roles delete viewer --project $PROJECT_ID

# カスタムの役割の削除を取り消す
gcloud iam roles undelete viewer --project $PROJECT_ID
```

## Service Account

```
# サービス アカウントを作成
gcloud iam service-accounts create my-sa-123 --display-name "my service account"

# サービス アカウントへ役割を付与
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member serviceAccount:my-sa-123@$PROJECT_ID.iam.gserviceaccount.com --role roles/editor
```


