C:\BooksDataフォルダを作成する。

mongoDBのデータを置きます。

ASP.NET Core と MongoDB で Web API を作成する
**1 MongoDB を構成する**
1.1  MongoDBをインストールする
　https://www.mongodb.com/dr/fastdl.mongodb.org/windows/mongodb-windows-x86_64-4.4.0-signed.msi/download
1.2　Windows を使用する場合、MongoDB は既定では C:\Program Files\MongoDB にインストールされます。 C:\Program Files\MongoDB\Server\<version_number>\bin を Path 環境変数に追加します。
1.3　データを格納するために開発用コンピューター上のディレクトリを選択します。 たとえば、Windows では C:\BooksData です。 存在しない場合はディレクトリを作成します。 mongo シェルでは新しいディレクトリは作成されません。
1.4　コマンド シェルを開きます。 次のコマンドを実行して、既定のポート 27017 で MongoDB に接続します。 忘れずに、前の手順で選択したディレクトリで <data_directory_path> を置き換えます。
1.5　
　コンソール
　mongod --dbpath <data_directory_path>
1.6　別のコマンド シェル インスタンスを開きます。 次のコマンドを実行して、既定のテスト データベースに接続します。
　コンソール
　mongo
1.7　コマンド シェルで次を実行します。
　コンソール
　use BookstoreDb
 これがまだ存在していない場合、BookstoreDb という名前のデータベースが作成されます。 データベースが存在する場合は、トランザクションのために接続されます。
1.8　次のコマンドを使用して Books コレクションを作成します。
　コンソール
　db.createCollection('Books')
 次のような結果が表示されます。
 コンソール
 { "ok" : 1 }
1.9 次のコマンドを使用して、Books コレクションのスキーマを定義し、2 つのドキュメントを挿入します。
　コンソール
  db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
 次のような結果が表示されます。
 コンソール
  {
  "acknowledged" : true,
  "insertedIds" : [
    ObjectId("5bfd996f7b8e48dc15ff215d"),
    ObjectId("5bfd996f7b8e48dc15ff215e")
  ]
}
1.10 次のコマンドを使用して、データベース内のドキュメントを表示します。
 コンソール
 db.Books.find({}).pretty()
 次のような結果が表示されます。
 {
  "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
  "Name" : "Design Patterns",
  "Price" : 54.93,
  "Category" : "Computers",
  "Author" : "Ralph Johnson"
}
{
  "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
  "Name" : "Clean Code",
  "Price" : 43.15,
  "Category" : "Computers",
  "Author" : "Robert C. Martin"
}
スキーマによって、自動生成された型が ObjectId の _id プロパティが各ドキュメントに追加されます。
データベースの準備ができました。 ASP.NET Core Web API の作成を開始できます。

**2 ASP.NET Core Web API プロジェクトを作成する**
2.1 [ファイル] > [新規作成] > [プロジェクト] の順にクリックします。
[ASP.NET Core Web アプリケーション] プロジェクトの種類を選択し、 [次へ] を選択します。
プロジェクトに BooksApi という名前を付けて、 [作成] を選択します。
[.NET Core] ターゲット フレームワークと [ASP.NET Core 3.0] を選択します。 [API] プロジェクト テンプレートを選択し、 [作成] を選択します。
NuGet ギャラリー:MongoDB.Driver に関するページを参照して、MongoDB 用 .NET ドライバーの最新の安定バージョンを確認します。 [パッケージ マネージャー コンソール] ウィンドウで、プロジェクトのルートに移動します。 次のコマンドを実行して、MongoDB 用の .NET ドライバーをインストールします。
  [.NET Core] :
  Install-Package MongoDB.Driver -Version {VERSION}
