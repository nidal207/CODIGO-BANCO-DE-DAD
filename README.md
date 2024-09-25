CREATE DATABASE supermercado;
USE supermercado;

CREATE TABLE produtos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    preco DECIMAL(10, 2) NOT NULL,
    quantidade INT NOT NULL,
    categoria_id INT,
    FOREIGN KEY (categoria_id) REFERENCES categorias(id)
);

CREATE TABLE categorias (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    telefone VARCHAR(15)
);

CREATE TABLE vendas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    data_venda TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE itens_venda (
    id INT AUTO_INCREMENT PRIMARY KEY,
    venda_id INT,
    produto_id INT,
    quantidade INT NOT NULL,
    preco DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (venda_id) REFERENCES vendas(id),
    FOREIGN KEY (produto_id) REFERENCES produtos(id)
);

INSERT INTO categorias (nome) VALUES ('Alimentos'), ('Bebidas'), ('Limpeza');

INSERT INTO produtos (nome, preco, quantidade, categoria_id) VALUES 
('Arroz', 20.50, 100, 1),
('Feijão', 10.00, 150, 1),
('Refrigerante', 5.00, 200, 2),
('Detergente', 3.50, 50, 3);

INSERT INTO clientes (nome, email, telefone) VALUES 
('Carlos Almeida', 'carlos@example.com', '123456789'),
('Ana Costa', 'ana@example.com', '987654321');

INSERT INTO vendas (cliente_id) VALUES (1);

INSERT INTO itens_venda (venda_id, produto_id, quantidade, preco) VALUES 
(1, 1, 2, 20.50),  -- 2 kg de Arroz
(1, 3, 1, 5.00);   -- 1 Refrigerante

SELECT v.id AS venda_id, c.nome AS cliente, v.data_venda, 
       p.nome AS produto, iv.quantidade, iv.preco
FROM vendas v
JOIN clientes c ON v.cliente_id = c.id
JOIN itens_venda iv ON v.id = iv.venda_id
JOIN produtos p ON iv.produto_id = p.id;

UPDATE produtos 
SET quantidade = quantidade - 2 
WHERE id = 1;  -- Exemplo: atualizando o estoque de arroz

