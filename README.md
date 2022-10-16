# Instalação Maven
https://www.appsdeveloperblog.com/how-to-install-maven-on-mac-os/

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install maven
mvn -v
```

# Criar projeto maven
- https://maven.apache.org/guides/mini/guide-creating-archetypes.html
- https://www.sohamkamani.com/java/cli-app-with-maven/

## Generate
- https://maven.apache.org/archetype/maven-archetype-plugin/generate-mojo.html

```shell
mvn archetype:generate                                  \
  -DinteractiveMode=<true|false>                        \ # mostra ou não mostra o modo iterativo(solicita confirmação dos dados da aplicação) 
  -DarchetypeArtifactId=<archetype-artifactId>          \ # tipo de projeto gerado - https://maven.apache.org/archetypes/
  -DarchetypeVersion=<archetype-version>                \ # versão para criação do tipo de projeto maven - https://stackoverflow.com/questions/55937171/mvn-archetypegenerate-which-version-selected
  -DgroupId=<my.groupid>                                \ # grupo do projeto que será utilizado no package
  -DartifactId=<my-artifactId>                            # nome do projeto
```

```shell
mvn archetype:generate                                  \
  -DinteractiveMode=false                               \
  -DarchetypeArtifactId=maven-archetype-quickstart      \
  -DarchetypeVersion=1.4                                \
  -DgroupId=br.com.maven_build                          \
  -DartifactId=maven_build 
```

## Build da aplicação
```shell
mvn validate # validar se o projeto está correto e todas as informações necessárias estão disponíveis
mvn compile # compila o código fonte da aplicação
mvn package # gera o jar da aplicação, rodando os testes de unidade
mvn clean # limpa a pasta target
mvn clean compile package -Dmaven.test.skip -DskipTests -Dmaven.javadoc.skip=true # compila e gera o pacote sem rodar os testes
mvn install # instala o pacote no repositório local, para uso como dependência em outros projetos localmente
mvn deploy #feito em um ambiente de integração ou lançamento, copia o pacote final para o repositório remoto para compartilhamento com outros desenvolvedores e projetos.
```

## Testes
```shell
mvn test # roda os testes da aplicação
```

## rodar
```shell
mvn exec:java -Dexec.mainClass="br.com.maven_build.App" # roda a aplicação apontando para a classe principal
```

## rodar em modo produção
```shell
java -jar target/maven_build-1.0-SNAPSHOT.jar # roda binário do java compilado
java -jar target/maven_build-1.0-SNAPSHOT.war # roda binário do java compilado

# para aplicações Spring Boot
./mvnw spring-boot:start # linux com Script "mvnw"
mvnw.cmd spring-boot:start # windows com Script "mvnw.cmd"
mvn spring-boot:start # Qualquer S.O. com mvn instalado
```

## Configurar o manifest (indicação para o jar qual é a classe inicial)
- https://www.sohamkamani.com/java/cli-app-with-maven/#running-our-code
```shell
vim pom.xml
```

### colocar o xml abaixo dentro da tag <build>
```xml
<!-- this goes within <build> -->
<plugins>
	<plugin>
		<!-- Build an executable JAR -->
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-jar-plugin</artifactId>
		<version>3.1.0</version>
		<configuration>
			<archive>
				<manifest>
					<addClasspath>true</addClasspath>
					<!-- here we specify that we want to use the main method within the App class -->
					<mainClass>br.com.maven_build.App</mainClass>
				</manifest>
			</archive>
		</configuration>
	</plugin>
</plugins>
<!-- other properties -->
```

### definindo versão java 17
### - dentro da tag <properties> colocar o xml abaixo
  
```shell
vim pom.xml
```

```xml
<properties>
    <java.version>17</java.version>
</properties>
```

# Adicionando dependencia
- https://mvnrepository.com/
```shell
<!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.3.19</version>
</dependency>
```
