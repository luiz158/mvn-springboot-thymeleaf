# MVC Application com Spring Boot

### Contents

* [Spring Boot](https://projects.spring.io/spring-boot/)
* [Spring MVC](https://spring.io/guides/gs/serving-web-content/)
* [Spring Data](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
* [JPA](https://pt.wikipedia.org/wiki/Java_Persistence_API)
* [Thymeleaf](http://www.thymeleaf.org/)
* [Maven](https://maven.apache.org/)
* [MySQL](https://www.mysql.com/)
* [Bootstrap 4](https://getbootstrap.com/)
* [HTML](https://pt.wikipedia.org/wiki/HTML)
* [Java 8 - Lambda](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)

-------------------

### [Developer tools][0]

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

### [Thymeleaf](http://www.thymeleaf.org/)
-------------

***References***

* [Serving Web Content with Spring MVC][1]
* [Spring Boot Hello World Example – Thymeleaf][2]
* [Tutorial: Thymeleaf + Spring][3]
* [Standard URL Syntax][6]
* [Concatenate strings in Thymeleaf][8]

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```


```html
<html xmlns:th="http://www.thymeleaf.org">

<h1 th:text="${titulo}">MVC Application</h1>

<tr th:each="pessoa : ${lista}">
	<td th:text="${pessoa.id}"></td>
	<td th:text="${pessoa.nome}"></td>
	<td>
		<a href="/editar">Editar</a>
		<a href="/deletar">Deletar</a>
	</td>
</tr>

<form action="#" th:action="@{/salvar}" th:object="${pessoa}" method="post">
	<label>ID</label>
	<input type="text" name="id" readonly="readonly" th:value="*{id}" />
	<br/>
	<br/>
	<label>Nome</label>
	<input type="text" name="nome" th:value="*{nome}" />
	<br/>
	<br/>
	<button type="submit">Salvar</button>
	<a href="/">Cancelar</a>
</form>

<a th:href="@{/editar/{id}(id=${pessoa.id})}">Editar</a>	

```

### [Java 8 - Lambda](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)

**Max/Min value**

[Finding Max/Min of a List or Collection][4]

```java
int max = lista.stream()
	.mapToInt(p -> p.getId())
	.max().orElse(0);

int max = lista.stream()
	.mapToInt(p -> p.getId())
	.min().orElse(0);		
```

**Filters**

[Java 8 Streams filter examples][5]


```java
Pessoa pessoa = lista.stream()
	.filter(p -> p.getId() == id)
	.findAny()
	.orElse(null);
```

**Update Value List**.  

```java
lista.forEach(p -> {
	if(p.getId() == pessoa.getId()){
		p.setNome(pessoa.getNome());
	}
});
```

**Delete Object from List**

[Reference][7]

```java
lista.removeIf(p -> p.getId() == id);
```

### Database

**References**

* [Accessing data with MySQL][9]

> Dependency

```xml
<!-- Database -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
</dependency>
```

> Properties

```properties
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/crudmvc
spring.datasource.username=crud
spring.datasource.password=123456
```

> MySQL

1. Create the new database
2. Creates the user
3. Gives all the privileges to the new user on the newly created database

```
mysql> create database crudmvc; 
mysql> create user 'crud'@'localhost' identified by '123456'; 
mysql> grant all on crudmvc.* to 'crud'@'localhost'; 
```

> Model 

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
public class Pessoa {

	@Id @GeneratedValue
	private int id;
	private String nome;

	// getters, setters, constructors and toString
}
```

> Repository

```java
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface PessoaRepository extends CrudRepository<Pessoa, Integer> {

}
```

### Bootstrap 4

**References**

* [Oficial Site][10]
* [Jumbotron][11]
* [Forms][12]
* [Tables][13]
* [Buttons][14]
* [Cards / Panels][15]
* [Navbar][16]
* [Containers][17]
* [Bootstrap 4 set table column width][18]

> Navbar

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item">
        <a class="nav-link" href="/">Home</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="/cadastro">Cadastro</a>
      </li>
      <li class="nav-item active">
        <a class="nav-link" href="/pesquisa">Pesquisa</a>
      </li>
     </ul>
  </div>
</nav>	
```

> Form

```html
<div class="card">
	<div class="card-body">
		<form action="#" th:action="@{/salvar}" th:object="${pessoa}" method="post">
			<label>ID</label>
			<input class="form-control" type="text" name="id" readonly="readonly" th:value="*{id}" />
			<label>Nome</label>
			<input class="form-control" type="text" name="nome" th:value="*{nome}" />
			<hr/>
			<button class="btn btn-primary" type="submit">Salvar</button>
			<a class="btn btn-secondary" href="/">Cancelar</a>
		</form>
	</div>
</div>	
```

> Table

```html
<div class="container-fluid">
  <div class="row card">
    <div class="col-sm-12">
	  <table class="table table-striped table-hover">
		<thead>
		  <tr class="row">
			<th class="col-sm-1 text-center">ID</th>
			<th class="col-sm-9">Nome</th>
			<th class="col-sm-2 text-center">
			  <a class="btn btn-primary" href="/cadastro">Novo</a>
			</th>
		  </tr>
		</thead>
		<tbody>
		  <tr class="row" th:each="pessoa : ${lista}">
			<td class="col-sm-1 text-center" th:text="${pessoa.id}"></td>
			<td class="col-sm-9" th:text="${pessoa.nome}"></td>
			<td class="col-sm-2 text-center">
			  <a class="btn btn-success" th:href="@{/editar/{id}(id=${pessoa.id})}">Editar</a>
			  <a class="btn btn-danger" th:href="@{/deletar/{id}(id=${pessoa.id})}">Deletar</a>
			</td>
		  </tr>
		</tbody>
	  </table>
    </div>
  </div>
</div>
```

-------------

### References All

[https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html#using-boot-devtools][0]
[https://spring.io/guides/gs/serving-web-content/][1]
[https://www.mkyong.com/spring-boot/spring-boot-hello-world-example-thymeleaf/][2]
[http://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html][3]
[http://www.baeldung.com/java-collection-min-max][4]
[https://www.mkyong.com/java8/java-8-streams-filter-examples/][5]
[http://www.thymeleaf.org/doc/articles/standardurlsyntax.html][6]
[https://www.leveluplunch.com/java/examples/remove-element-from-list/][7]
[https://gist.github.com/romach/10081ba3e24ffc9f75aadada7df80df8][8]
[https://spring.io/guides/gs/accessing-data-mysql/][9]
[http://getbootstrap.com/][10]
[https://getbootstrap.com/docs/4.0/components/jumbotron/][11]
[https://getbootstrap.com/docs/4.0/components/forms/][12]
[https://getbootstrap.com/docs/4.0/content/tables/][13]
[https://getbootstrap.com/docs/4.0/components/buttons/][14]
[https://getbootstrap.com/docs/4.0/components/card/][15]
[https://getbootstrap.com/docs/4.0/components/navbar/][16]
[https://getbootstrap.com/docs/4.0/layout/overview/#containers][17]
[https://www.codeply.com/go/OkCUy1M8JM/bootstrap-4_alpha-6-set-table-column-width][18]


[0]: https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html#using-boot-devtools
[1]: https://spring.io/guides/gs/serving-web-content/
[2]: https://www.mkyong.com/spring-boot/spring-boot-hello-world-example-thymeleaf/
[3]: http://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html
[4]: http://www.baeldung.com/java-collection-min-max
[5]: https://www.mkyong.com/java8/java-8-streams-filter-examples/
[6]: http://www.thymeleaf.org/doc/articles/standardurlsyntax.html
[7]: https://www.leveluplunch.com/java/examples/remove-element-from-list/
[8]: https://gist.github.com/romach/10081ba3e24ffc9f75aadada7df80df8
[9]: https://spring.io/guides/gs/accessing-data-mysql/
[10]: http://getbootstrap.com/
[11]: https://getbootstrap.com/docs/4.0/components/jumbotron/
[12]: https://getbootstrap.com/docs/4.0/components/forms/
[13]: https://getbootstrap.com/docs/4.0/content/tables/
[14]: https://getbootstrap.com/docs/4.0/components/buttons/
[15]: https://getbootstrap.com/docs/4.0/components/card/
[16]: https://getbootstrap.com/docs/4.0/components/navbar/
[17]: https://getbootstrap.com/docs/4.0/layout/overview/#containers
[18]: https://www.codeply.com/go/OkCUy1M8JM/bootstrap-4_alpha-6-set-table-column-width
