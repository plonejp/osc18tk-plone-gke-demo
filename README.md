# osc18tk-plone-gke-demo
OSC 2018 Tokyo Spring で行ったPloneのGKE(Kubernetes)デモ


# GKEデプロイ(app)

以下は、GKEのWebシェルで実行可能

## レポジトリのダウンロード

```
$ git clone https://github.com/plonejp/osc18tk-plone-gke-demo.git
$ cd osc18tk-plone-gke-demo
$ git pull
```

## DBパスワードの設定

```
$ vi app/site.cfg

password <YOUR-DB-PASSWORD>
```

## コンテナ作成

```
$ export PROJECT_ID="osc-plone-test-rel"
$ export VERSION_NO="v2.0"

$ docker build -t osc-plone:$VERSION_NO app/

$ docker tag osc-plone:$VERSION_NO gcr.io/$PROJECT_ID/osc-plone:$VERSION_NO
$ gcloud docker -- push gcr.io/$PROJECT_ID/osc-plone:$VERSION_NO

$ gcloud container clusters get-credentials plone-cluster-1 --zone=asia-northeast1-a
```

## バージョン等変更箇所

app-deployment.yaml のコンテナバージョンを常に変更する必要がある。

## 起動

```
$ kubectl create -f config/app-deployment.yaml
$ kubectl create -f config/app-service.yaml

$ kubectl get services
```

## 停止

```
$ kubectl delete service app-service
$ kubectl delete deployment app-node
```

## 更新

```
$ gcloud container clusters get-credentials plone-cluster-1 --zone=asia-northeast1-a
$ kubectl apply -f config/app-deployment.yaml
```

## 確認

```
$ kubectl get pods
```


# GKEデプロイ(front)

以下は、GKEのWebシェルで実行可能


## レポジトリのダウンロード

```
$ git clone https://bitbucket.org/cmscom/plone-gke-demo.git
$ cd plone-gke-demo
$ git pull
```

## コンテナ作成

```
$ export PROJECT_ID="osc-plone-test-rel"
$ export FRONT_VERSION_NO="v1.1"

$ docker build -t osc-front:$FRONT_VERSION_NO front/

$ docker tag osc-front:$FRONT_VERSION_NO gcr.io/$PROJECT_ID/osc-front:$FRONT_VERSION_NO
$ gcloud docker -- push gcr.io/$PROJECT_ID/osc-front:$FRONT_VERSION_NO

$ gcloud container clusters get-credentials plone-cluster-1 --zone=asia-northeast1-a
```

## バージョン等変更箇所

front-deployment.yaml のコンテナバージョンを常に変更する必要がある。

## 起動

```
$ kubectl create -f config/front-deployment.yaml
$ kubectl create -f config/front-service.yaml
```

```
$ kubectl get services
$ kubectl get pods
```
