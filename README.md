#### <u>Projeto final do boot camp Linux Experience</u>



> Nesse projeto iremos criar um conjunto de contêineres com ajuda do docker que conversarão entre si, trocando dados e informações importantes pra o funcionando do swarm.

Após criado 3 maquinas na AWS, usares a seguinte configuração uma das maquinas será o servidor pode onde iremos controlar as demais e as outras serão os Workers.



#### <u>Requerimentos</u>

Iremos precisar de três maquinas virtuais no seu pc ou em algum servidor cloud, sendo uma o servidor que ira controlar outras duas que serão os workers.

No meu caso irei criar um criar três maquinas virtuais na AWS, as controlaremos via uma distribuição Fork do ubuntu o Zorin OS.

Após criada três maquinas virtuais, realizamos a atualização do sistema nas três maquinas com os comandos a seguir;



*sudo apt-get update*

*sudo apt-get upgrade*



Feito os comando acima nas tres maquinas iremos realizar a instalação do mysql-server na maquina servidor com o seguinte comando;



sudo apt install mysql-server



Apos instalado o MySQL
Para facilitar o acesso remoto ao banco de dados
Devemos trocar a forma de autenticação de auth_socket para o modo de senha

Você pode usar o link abaixo para entender melhor o procedimento

Link https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04-pt

Também é necessário abrir a porta de acesso ao banco de dados criado.

Criaremos uma tabela qual quer somente para teste você pode seguir o passo a passo a baixo.

CREATE TABLE dados (
    AlunoID int,
    Nome varchar(50),
    Sobrenome varchar(50),
    Endereco varchar(150),
    Cidade varchar(50),
    Host varchar(50)
);





Criaremos uma pagina html para fazer conexão com o mysql

você pode usar o exemplo abaixo lembrando sempre de editar os valores de acordo com o seu ip e usuarios e senhas





<html>

<head>
<title>Exemplo PHP</title>
</head>

<body>

<?php
ini_set("display_errors", 1);
header('Content-Type: text/html; charset=iso-8859-1');



echo 'Versao Atual do PHP: ' . phpversion() . '<br>';

$servername = "54.234.153.24";
$username = "root";
$password = "Senha123";
$database = "meubanco";

// Criar conexão


$link = new mysqli($servername, $username, $password, $database);

/* check connection */
if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

$valor_rand1 =  rand(1, 999);
$valor_rand2 = strtoupper(substr(bin2hex(random_bytes(4)), 1));
$host_name = gethostname();


$query = "INSERT INTO dados (AlunoID, Nome, Sobrenome, Endereco, Cidade, Host) VALUES ('$valor_rand1' , '$valor_rand2', '$valor_rand2', '$valor_rand2', '$valor_rand2','$host_name')";


if ($link->query($query) === TRUE) {
  echo "New record created successfully";
} else {
  echo "Error: " . $link->error;
}

?>
</body>
</html>





Execute o comando docker run para iniciar container e ter acesso ao serviço

Podemos criar um docker Swarm onde teremos varios container rodando juntos

Após iniciar o docker swarm com o comando docker Init e adicione os workes com comando docker swarm join.



Como o comando docker node ls você consegue listar todas as maqunas vinculadas ao seu swarm



Para criar um serviço 





