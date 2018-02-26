> /Applications/mampstack-7.1.4-0/mysql

이 디렉토리에 mysql이 존재한다.

> cd bin

> sudo ./mysql -u root -p

이렇게 입력하면 터미널에서 mysql 서버에 접근 가능하며, 테이블 조회 등등 여러가지를 할 수 있다.

### connection 생성

```javascript
mysql.createConnection({
    host: config.host,
    port: config.port,
    user: config.user,
    password: config.password,
    database: config.database
})
```

### 데이터 조회

```javascript
connection.connect();
connection.query('SELECT * from Persons', function(err, rows, fields) {
    if (err) {
        console.log('Error while performing Query.' , err);
    }else {
        console.log('The solution is: ' + JSON.stringify(rows));
    }
});
connection.end();
```

데이터 조회시 end를 꼭 해서 connection 관리를 잘 해줘야 app이 db때문에 죽지 않는다.
