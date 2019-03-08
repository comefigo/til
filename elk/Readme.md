# 概要

初めてELK環境を構築するのにあたり、学んだことをまとめたいと思います

# ELK(Elasticsearch、Logstash、Kibana)とは

オランダのElastic社によって、開発・メンテナンスされているOSSソフトウエア

## Elasticsearch

Restfulな分散型全文検索エンジン
検索コアは[Apache Lucene](https://ja.wikipedia.org/wiki/Apache_Lucene)を用いている
Javaで実装されている

## Logstash

データの集約（Input）と加工（Filter）と転送（Output）を担うサーバーサイドデータ処理パイプライン

## Kibana

ElasticsearchのデータをビジュアライゼーションするWebアプリケーション
Node.jsで実装されている

# 環境作成

バージョン5.0から製品バージョンをそろえてリリースされるようになったので、バージョンを揃えるように構築しましょう