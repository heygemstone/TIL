<!--k03_김민/ 2018.06.25.-->
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<%@ page contentType="text/html; charset=utf-8" %>
<%@ page import="java.sql.*,javax.sql.*,java.io.*" %>

<%
request.setCharacterEncoding("utf-8");
//입력값 한글 인코딩 될 수 있도록 설정
String name = request.getParameter("name");
//문자열 변수 k03_name의 값은 request 객체 사용하여 "username"의 값을 받아옴

String k03_studentid = request.getParameter("studentid");
if (k03_studentid == null) {
    k03_studentid = "0";
}
int studentid = Integer.parseInt(k03_studentid);

String k03_kor = request.getParameter("kor");
if (k03_kor == null) {
    k03_kor = "0";
} //값이 없으면 0으로 저장합니다. 
int kor = Integer.parseInt(k03_kor);

String k03_eng = request.getParameter("eng");
if (k03_eng == null) {
    k03_eng = "0";
}
int eng = Integer.parseInt(k03_eng);

String k03_mat = request.getParameter("mat");
if (k03_mat == null) {
    k03_mat = "0";
}
int mat = Integer.parseInt(k03_mat);

//문자열 변수 k03_password 값은 request 객체 사용하여 "userpasswd"의 값을 받아옴
%>
<%
Class.forName("com.mysql.jdbc.Driver");
		// "com.mysql.jdbc.Driver" 클래스가 메모리에 로드됨
		// 필요없는 기능이므로 주석처리해도 실행 가능
Connection k03_conn = DriverManager.getConnection("jdbc:mysql://localhost/kopo03","root","koposw");
Statement k03_stmt = k03_conn.createStatement();
		// Statement -> select, create 등을 실행함
		// Query : SQL 명령문 자바에서도 실행가능하도록 함

int newStudentID = 0;
String k03_Query_1 = "select max(studentid) from examtable";
ResultSet k03_rset= k03_stmt.executeQuery(k03_Query_1);

while(k03_rset.next()) {
newStudentID = k03_rset.getInt(1) + 1;
//out.println(newStudentId);
}

String k03_Query_2 = "insert into examtable (name, studentid, kor, eng, mat)" + 
        "values ('" + name + "', " + newStudentID + ", " + kor + ", " + eng + ", " + mat + ");";
		k03_stmt.execute(k03_Query_2);
		// insert into 테이블명(필드) values(값); : 테이블에 record 생성
		// Statement 객체.excute() 메소드 통하여 mysql에 똑같은 명령문 실행하도록 함(create=insert)

		k03_stmt.close();
		k03_conn.close();

%>
<HTML>
<HEAD>
  <TITLE></TITLE>
  <%-- 페이지 제목 '로그인'으로 설정 --%>
</HEAD>

<BODY>
    <strong>
        <font size=7>
            성적입력추가완료
        </font>
    </strong>
    <br>
    <br>
    <form method="post" action="inputForm1.html">
        <table border=0 width=600 cellsapcing=1>
            <tr>
                <td align=right>
                    <input type="submit" value="뒤로가기">
                </td>
            </tr>
        </table>
        <table border=1 width=600 cellspacing=1>
            <tr>
                <td>
                    <center>
                        <p align=center>이름</p>
                    </center>
                </td>
                <td>
                    <center>
                        <!-- Post방식 : html페이지에 form 형태에서 값을 전달 / member.jsp 페이지에서 실행-->
                        <input type="text" name="name" value="<%= name %>">
                        <!-- 이름의 값은 text로 받으며 필드명은 username -->
                    </center>
                </td>
                </center>
            </tr>
            <tr>
                <td>
                    <p align=center>학번</p>
                </td>
                <td>
                    <center>
                        <!-- Post방식 : html페이지에 form 형태에서 값을 전달 / member.jsp 페이지에서 실행-->
                        <input type="text" name="name" value="<%= newStudentID %>">
                        <!-- 이름의 값은 text로 받으며 필드명은 username -->
                    </center>
                </td>
            </tr>
            <tr>
                <td>
                    <center>
                        <p align=center>국어</p>
                    </center>
                </td>
                <td>
                    <center>
                        <!-- Post방식 : html페이지에 form 형태에서 값을 전달 / member.jsp 페이지에서 실행-->
                        <input type="text" name="kor" value="<%= kor %>">
                        <!-- 이름의 값은 text로 받으며 필드명은 username -->
                    </center>
                </td>
            </tr>
            <tr>
                <td>
                    <center>
                        <p align=center>영어</p>
                    </center>
                </td>
                <td>
                    <center>
                        <!-- Post방식 : html페이지에 form 형태에서 값을 전달 / member.jsp 페이지에서 실행-->
                        <input type="text" name="eng" value="<%= eng %>">
                        <!-- 이름의 값은 text로 받으며 필드명은 username -->
                    </center>
                </td>
            </tr>
            <tr>
                <td>
                    <center>
                        <p align=center>수학</p>
                    </center>
                </td>
                <td>
                    <center>
                        <!-- Post방식 : html페이지에 form 형태에서 값을 전달 / member.jsp 페이지에서 실행-->
                        <input type="text" name="mat" value="<%= mat %>">
                        <!-- 이름의 값은 text로 받으며 필드명은 username -->
                    </center>
                </td>
            </tr>
        </table>
    </form>

</BODY>

</HTML>
