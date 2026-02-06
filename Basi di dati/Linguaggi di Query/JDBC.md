>[!note]
>Java Database Connectivity (JDBC) è un API standard Java che permette alle applicazioni di connettersi, effettuare interrogazioni, e aggiornare database relazionali.
>
>Per connettersi ad un database si usa il codice:
>```java
>import java.sql.*;
>
>Connection con = DriverManager.getConnection(
>	"jdbc:mysql://localhost:3306/world",
>	"root", "password"
>);
>
>// varie operazioni
>
>con.close();
>```
>
>Il codice JDBC può lanciare eccezioni del tipo `SQLException`.

Per eseguire una query si può usare il codice:
```java
String query = "SELECT Name, Population FROM City";
Statement stmt = con.createStatement();
ResultSet rs = stmt.executeQuery(query);

while(rs.next()) {
	String nome = rs.getString("Name");
	int pop = rs.getInt("Population");
	System.out.println(nome + "-" + pop);
}

rs.close();
stmt.close();
```

Per eseguire un insert/update/delete si usa;
```java
Statement stmt = con.createStatement();
stmt.executeUpdate(
	"INSERT INTO City(Name,CountryCode,District,Population) " +
	"VALUES('Trantor', 'ITA', 'Lombardia', 50000)"
):
stmt.close();
```

>[!tip] Procedure
>Un esempio di procedura può essere:
>```java
>try {
>	con.setAutoCommit(false);
>	
>	Statement st = con.createStatement();
>	
>	st.executeUpdate("UPDATE country SET Population = Population + 1000 WHERE Code = 'ITA'");
>	st.executeUpdate("UPDATE country SET Population = Population - 1000 WHERE Code = 'FRA'");
>	
>	con.commit();
>} catch(Exception ex) {
>	try {
>		con.rollback();
>	} catch(SQLException sqx) {
>		sqx.printStackTrace();
>	}
>}
>```

>[!tip] Prepared Statement
>È possibile inserire valori negli statement dopo la loro creazione:
>```java
>String insertSql="INSERT INTO city (Name, CountryCode, District, Population) VALUES (?, ?, ?, ?)";
>PreparedStatement insertStmt = connection.prepareStatement(insertSql);
>
>insertStmt.setString(1, "Trantor");
>insertStmt.setString(2, "ITA");
>insertStmt.setString(3, "Lombardia");
>insertStmt.setInt(4, 60000);
>
>int rowsInserted = insertStmt.executeUpdate();
>System.out.println("Trantor inserita con successo (" + rowsInserted + " riga).");
>```

### Connection Pool
>[!note]
>Una connection pool è una lista di connessioni a database riutilizzabili dal client o dal middleware. Riduce l'overhead della chiusura ed apertura di connessioni, migliorando velocità e scalabilità nelle applicazioni che fanno uso di database.

Di seguito un implementazione in JDBC di una connection pool:
```java
import java.sql.*;
import java.util.*;

public class PooledConnection {
	private Connection connection;
	private boolean busy;
	private String who;
	
	public PooledConnection(Connection connection) {
		this.connection = connection;
		this.busy = false;
		this.who = "";
	}
	
	public Connection getConnection() {
		return connection;
	}
	
	public boolean isBusy() {
		return busy;
	}
	
	public void setBusy(boolean busy) {
		this.busy = busy;
	}
	
	public String getWho() {
		return who;
	}
	
	public void setWho(String who) {
		this.who = who;
	}
}

public class ConnectionPool {
	private List<PooledConnection> pool;
	private int numCon;
	private int inc;
	
	private String dbUrl;
	private String dbUsername;
	private String dbPswd;
	private String driverString;
	
	public ConnectionPool(String dbUrl, String dbUsername, String dbPswd, int numCon, int inc, String, driverString) throws Exception {
		this.dbUrl = dbUrl;
		this.dbUsername = dbUsername;
		this dbPswd = dbPswd;
		this.numCon = numCon;
		this.inc = inc;
		this.driverString = driverString;
		
		this.pool = new ArrayList<>();
		newConnections(numCon);
	}
	
	private synchronized void newConnections(int count) throws SQLException {
		for (int i = 0; i < count; i++) {
			Connection conn = DriverManager.getConnection(dbUrl, dbUsername, dbPswd);
			pool.add(new PooledConnection(conn));
		}
	}
	
	public synchronized Connection getConnection(String who) throws Exception {
		for (PooledConnection pc : pool) {
			if (!pc.isBusy()) {
				pc.setBusy(true);
				pc.setWho(who);
				return pc.getConnection();
			}
		}
		
		newConnections(inc);
		return getConnection(who);
	}
	
	public synchronized Connection getConnection() throws Exception {
		return getConnection("noName");
	}
	
	public synchronized void releaseConnection(Connection c) {
		for (PooledConnection pc : pool) {
			if (pc.getConnection() == c) {
				pc.setBusy(false);
				pc.setWho("");
				break;
			}
		}
	}
	
	public String printStatusConnection() {
		StringBuilder sb = new StringBuilder();
		int i = 0;
		for (PooledConnection pc : pool) {
			sb.append("Connessione ")
				.append(i++).append(": ").append(pc.isBusy())
				.append(" used by: ").append(pc.getWho()).append("\n");
		}
		return sb.toString();
    }
}
```