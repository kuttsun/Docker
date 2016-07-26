# ベースイメージ
# GitBucketの動作に必要なのはJavaだけなので、javaのjreをベースとする
FROM java:9-jre

# 作者
MAINTAINER kuttsun

# ファイルの追加
ADD https://github.com/gitbucket/gitbucket/releases/download/4.2/gitbucket.war /opt/gitbucket.war

# コマンドの実行
# シンボリックリンクの作成(/gitbucket へのリンクを /root/.gitbucket に作成)
# Dockerでコンテナを起動すると、デフォルトはrootになる
# GitBucketはhomeディレクトリの .gitbucket ディレクトリに設定やリポジトリのデータが入る
# 従って、この場合、/root/.gitbucket に設定やリポジトリのデータが入ることになる
# ここでは、/root/.gitbucket を /gitbucket のシンボリックリンクにしているので、
# 実際には /gitbucket 設定やリポジトリのデータが入ることになる
RUN ln -s /gitbucket /root/.gitbucket

# ちなみにDockerは RUNコマンドを実行するたびに、イメージの「レイヤー」を保存して、
# その状態をイメージにキャッシュする。
# 最後に削除しても、途中でレイヤーにコミットしちゃっているから、結局イメージのサイズがそのまま大きくなる
# 従って、コマンドを複数実行する場合は、１つのRUNコマンドで全部実行するようにしないと
# イメージサイズが大きくなってしまうらしい

# データボリュームを指定する(マウントポイントの作成)
# これを指定しないと、データの保存が行えないため、gitbucketは初回起動時のデータ生成が行えず起動エラーとなる
# また、docker run が -vオプションを使用して、他のコンテナの領域をここにマウントできるようになる
VOLUME /gitbucket

# Port for web page
# GitBucketはデフォルトで8080のポートを使用するので、8080のポートを開けておく
EXPOSE 8080

# Port for SSH access to git repository (Optional)
EXPOSE 29418

# デーモン実行
CMD ["java", "-jar", "/opt/gitbucket.war"]

