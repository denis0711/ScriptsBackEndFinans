# ScriptsBackEndFinans

```sql

SELECT * FROM Cartoes

select * from Produtos

select len('286d53b5-69ce-4bbd-b3f4-b4275d64adc4')
  

daddasd

CREATE TABLE Pedido
(
	CODIGO_PEDIDO VARCHAR(50)  primary key,
	CODIGO_PRODUTO INT

)


CREATE TABLE PedidoStatus
(
	STATUS INT IDENTITY(1,1) primary key,
	DESCRICAO VARCHAR(30)
)

CREATE TABLE AvalicaoProduto
(
	CODIGO_PRODUTO INT,
	ESTRELA_UM INT,
	ESTRELA_DOIS INT,
	ESTRELA_TRES INT,
	ESTRELA_QUATRO INT,
	ESTRELA_CINCO INT,
)

CREATE TABLE AvaliacaoPedido
(
	CODIGO_PEDIDO VARCHAR(50) NOT NULL,
	COMENTARIO VARCHAR(300),
	CODIGO_NOTA INT NOT NULL,
	DATA DATE,
)



--ALTERS NECESSÁRIOS
ALTER TABLE Produtos ADD IMAGEM VARCHAR(MAX)


--UPDATES NECESSÁRIOS
update Produtos set IMAGEM = 'https://i.postimg.cc/VkpHWSLK/xburger.jpg'

SELECT * FROM Produtos

--INSERTS NECESSÁRIOS
INSERT INTO AvalicaoProduto (CODIGO_PRODUTO, ESTRELA_CINCO)
values
(2,1),
(3,1),
(4,1),
(5,1),
(6,1),
(7,1),
(8,1)


UPDATE  AvalicaoProduto SET ESTRELA_QUATRO = ESTRELA_QUATRO + 1 WHERE CODIGO_PRODUTO = 2
values
(2,1)

SELECT * FROM AvalicaoProduto

INSERT NotaPedido VALUES (1, '')

--NÃO VAI TER STATUS
--INSERT INTO PedidoStatus values('Criado'),('Preparando'),('Finalizado')
 -- LOGICA AVALIACAO RRODUTO SELECT (1.0*1 + 2.0*4 + 3.0*1 + 4.0*1 + 5.0*10) / (1 + 4 + 1 + 1 + 10)

 
 SELECT * FROM AvalicaoProduto

 ---- CARDAPIO EL MAGO BURGUERS


 SELECT A.IMAGEM 
      , A.PRODUTO
	  , A.VALOR
	  ,(1.0*ISNULL(B.ESTRELA_UM,0) + 2.0*ISNULL(B.ESTRELA_DOIS,0) + 3.0*ISNULL(B.ESTRELA_TRES,0) + 4.0*ISNULL(B.ESTRELA_QUATRO,0) + 5.0*ISNULL(B.ESTRELA_CINCO,0))
	  / (ISNULL(B.ESTRELA_UM,0) + ISNULL(B.ESTRELA_DOIS,0) + ISNULL(B.ESTRELA_TRES,0) + ISNULL(B.ESTRELA_QUATRO,0) + ISNULL(B.ESTRELA_CINCO,0)) Avaliacao
	  , (B.ESTRELA_UM+B.ESTRELA_DOIS+B.ESTRELA_TRES+B.ESTRELA_DOIS+B.ESTRELA_TRES+B.ESTRELA_QUATRO+B.ESTRELA_CINCO) AS VotosTotais
   FROM Produtos A
   LEFT JOIN AvalicaoProduto B ON A.CODIGO_PRODUTO = B.CODIGO_PRODUTO
  WHERE A.CODIGO_PRODUTO <> 1
  GROUP BY A.IMAGEM 
      , A.PRODUTO
	  , A.VALOR
	  , B.ESTRELA_CINCO
	  , B.ESTRELA_DOIS
	  , B.ESTRELA_QUATRO
	  , B.ESTRELA_TRES
	  , B.ESTRELA_DOIS
	  , B.ESTRELA_UM
  

---- AVALIACAO DO PEDIDO


CREATE PROCEDURE AVALIAR_PEDIDO_SP
@CODIGO_PEDIDO VARCHAR(50),
@COMENTARIO VARCHAR(300) = NULL,
@NOTA INT

AS
BEGIN
	
	DECLARE @DATE DATE = CONVERT(DATE,GETDATE())

	IF((SELECT COUNT(1) FROM AvaliacaoPedido WHERE CODIGO_PEDIDO = @CODIGO_PEDIDO) > 0)
	BEGIN
				INSERT INTO AvaliacaoPedido values(@CODIGO_PEDIDO,@COMENTARIO,@NOTA,@DATE)


				IF(@NOTA = 1)
				BEGIN
					SELECT 'Desculpe pelo Mal estar iremos fazer tudo para melhorar! Obrigado Pelo feedback!' AS Mensagem
				END
				IF(@NOTA = 2)
				BEGIN
					SELECT 'Desculpe por isso iremos fazer tudo para melhorar! Obrigado Pelo feedback!' AS Mensagem
				END
				IF(@NOTA = 3)
				BEGIN
					SELECT 'Obrigado Pelo feedback, Iremos melhorar sempre para satisfazer todos nossos clientes!' AS Mensagem
				END
				IF(@NOTA = 4)
				BEGIN
					SELECT 'Obrigado Pelo feedback, Fico feliz que você tenha gostamos vamos melhorar sempre!' AS Mensagem
				END
				IF(@NOTA = 5)
				BEGIN
					SELECT 'Muito Obrigado mesmo, Iremos sempre melhorar para sempre sejamos avaliados assim' AS Mensagem
				END
	  END
	  ELSE
	  BEGIN
			SELECT 'Pedido ja foi avaliado Obrigado Pelo FeedBack!' AS Mensagem
	  END
END

CREATE PROCEDURE MOSTRAR_PRODUTOS_PEDIDO_SP
@CODIGO_PEDIDO VARCHAR(50)
AS
BEGIN
	 	 SELECT A.IMAGEM 
			  , A.PRODUTO
			  , A.VALOR
	       FROM Produtos A
      LEFT JOIN Pedido B ON A.CODIGO_PRODUTO = B.CODIGO_PRODUTO
          WHERE A.CODIGO_PRODUTO <> 1 AND B.CODIGO_PEDIDO = @CODIGO_PEDIDO
END



CREATE PROCEDURE AVALIACAO_PRODUTO_SP
@CODIGO_PRODUTO INT,
@ESTRELA INT
AS
BEGIN
	 IF(@ESTRELA = 1 )
		UPDATE  AvalicaoProduto SET ESTRELA_UM = ESTRELA_UM + 1 WHERE CODIGO_PRODUTO = @CODIGO_PRODUTO

	 IF(@ESTRELA = 2 )
		UPDATE  AvalicaoProduto SET ESTRELA_DOIS = ESTRELA_DOIS + 1 WHERE CODIGO_PRODUTO = @CODIGO_PRODUTO
		
	 IF(@ESTRELA = 3 )
		UPDATE  AvalicaoProduto SET ESTRELA_TRES = ESTRELA_TRES + 1 WHERE CODIGO_PRODUTO = @CODIGO_PRODUTO
		
	 IF(@ESTRELA = 4 )
		UPDATE  AvalicaoProduto SET ESTRELA_QUATRO = ESTRELA_QUATRO + 1 WHERE CODIGO_PRODUTO = @CODIGO_PRODUTO
		
	 IF(@ESTRELA = 5 )
		UPDATE  AvalicaoProduto SET ESTRELA_CINCO = ESTRELA_CINCO + 1 WHERE CODIGO_PRODUTO = @CODIGO_PRODUTO
	
END






SELECT * FROM GASTOS

ALTER TABLE Gastos
ADD CATEGORIA_GASTO INT 


CREATE TABLE CATEGORIA_GASTOS
(
	CODIGO_GASTO INT IDENTITY(1,1) PRIMARY KEY,
	DESCRICAO VARCHAR(25)
)


INSERT CATEGORIA_GASTOS VALUES('RENOVAR ESTOQUE'),('EMERGÊNCIAS'),('SALARIOS'),('MELHORIAS')



UPDATE A
   SET A.CATEGORIA_GASTO = 1
  FROM Gastos A


CREATE PROCEDURE CATEGORIA_GASTOS_SP_S
AS
BEGIN
	SELECT A.CODIGO_GASTO AS CodigoGasto
	     , A.DESCRICAO AS Descricao
	  FROM CATEGORIA_GASTOS A
END


GO
 ALTER PROCEDURE [dbo].[GASTO_SP_I] --GASTO_SP_I @VALOR = '500.00', @OBS = 'Salarios dos Funcionàrios', @CODIGO_GASTO=3
			@VALOR DECIMAL(18,2),
			@OBS VARCHAR(MAX),
			@CODIGO_GASTO INT
		  AS
		  BEGIN
			   
				INSERT INTO Gastos VALUES(@VALOR,GETDATE(),@OBS,@CODIGO_GASTO);
			  
		  END






```
