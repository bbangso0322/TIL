# Java Database Connectivity
프로그래밍 언어 Java에서 DB 프로그래밍을 하기 위해 사용되는 API  


## 기본적인 사용법
설치 및 연동과 함께 data를 수정, 조회하는 기본적인 조작방법을 알아본다.
<br/><br/>

### 설치 및 연동
먼저, DBMS를 설치한다. 본 문서에서는 MYSQL을 기준으로 설명한다. 또한 이클립스를 사용한다.

MYSQL을 설치할 시, `connetor동를 선택하여 같이 설치한다. Windows 환경에서 설치했다면 일반적으로 다음과 같은 경로에 설치 된다. 
```bash
C:\Program Files (x86)\MySQL\Connector J 8.0
```

이클립스를 실행한 뒤 `src`밑에 `libs`란 폴더를 만들고 위 경로에 있는 `connector`를 복사하여 폴더에 넣는다. 복사된 파일을 오른쪽 클릭한 뒤 `build path`, `Add to Build Path`를 클릭한다. 정상적으로 되었다면 `Referenced Libraries`에 build 된 것을 볼 수 있다. 이제 mysql를 동작시킬 라이브러리를 사용할 수 있게 되었다.  
<br/>

### 기본적인 조작
이제 JDBC 라이브러리의 사용법을 익혀보자. 다음과 같은 코드를 통해 DBMS에 접속하고 sakila DB에 접근해 데이터를 추가, 조회 할 수 있다.
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;


public class jdbc1 {
	public static void main(String[] args) {
		Connection con = null;
		Statement st = null;
		ResultSet rs = null;
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");  //드라이버 로드
			con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/sakila?serverTimezone=UTC&useUniCode=yes&characterEncoding=UTF-8","root","root123!");  // MYSQL DBMS의 sakila DB에 접속. 이 예제에서는 DBMS의 아이디 root, 비밀번호 root123
			st = con.createStatement();  // 쿼리 객체
			rs = st.executeQuery("select * from actor");  // 쿼리수행 후 결과 받기
			
			while(rs.next()) {  // .next() method를 통해 결과값 순회
				System.out.println(rs.getString("first_name") + " , " +rs.getInt("actor_id"));
			}
			String sql = "insert into actor(first_name, last_name) values('SOYUN', 'BBANG')";
			st.execute(sql);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			// e.printStackTrace();
			System.out.println("class loading fail");
		}
		finally {
			try {
				if(rs!=null) rs.close();
				if(st!=null) st.close();
				if(con!=null) con.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
}

```
<br/>

다음과 같이 `prepareStatement` 객체를 이용하면 다이나믹한 sql문을 구성할 수 있다. 

```java
...

String f_name = "soyun";
String l_name = "bang";

String sql = "insert into actor(first_name, last_name) values(?, ?)";
PreparedStatement pSt = con.prepareStatement(sql);
pSt.setString(1, f_name); // 1번째 물음표에 f_name을 집어 넣는다.
pSt.setString(2, l_name); // 2 ''
pSt.executeUpdate();

...
```
<br/>

쿼리 결과의 column의 수, 이름 등과 같은 metadata를 다음과 같이 확인 할 수 있다.
```java
...

st = con.createStatement();
rs = st.executeQuery("select * from actor");
ResultSetMetaData metaData = rs.getMetaData();

int cnt = metaData.getColumnCount();
System.out.println(cnt);

for(int i=1; i<=cnt; i++) {
	System.out.print(metaData.getColumnName(i)+ " ");
}

...
```
<br/>
