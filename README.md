# sql
CREATE TABLE A_Alibis (
    id_alibi INT PRIMARY KEY,
    id_suspeito INT,
    descricao_alibi TEXT NOT NULL,
    confirmado TINYINT DEFAULT 0,
    CONSTRAINT chk_id_alibi CHECK (id_alibi > 0),
    FOREIGN KEY (id_suspeito) REFERENCES P_Suspeitos(id_suspeito)
);

-- Inserir o álibi para o Agente Gama (id 3)
INSERT INTO A_Alibis (id_alibi, id_suspeito, descricao_alibi, confirmado)
VALUES (1, 3, 'Estava em reunião com a diretoria durante o horário do
incidente.', 0);

-- Atualizar o nível de perigo da pista corrompida (id 105) para 7
UPDATE P2_Pistas
SET nivel_perigo = 7
WHERE id_pista = 105;

-- Deletar a pista falsa (id 102)
DELETE FROM P2_Pistas
WHERE id_pista = 102;






SELECT
    s.departamento,
    COUNT(p.id_pista) AS total_pistas
FROM P_Suspeitos s
INNER JOIN P2_Pistas p ON s.id_suspeito = p.id_suspeito
GROUP BY s.departamento
HAVING COUNT(p.id_pista) > 1;


SELECT nome
FROM P_Suspeitos
WHERE id_suspeito IN (
    SELECT id_suspeito
    FROM P2_Pistas
    WHERE nivel_perigo = (SELECT MAX(nivel_perigo) FROM P2_Pistas)
);


WITH SuspeitosTI AS (
    SELECT nome AS info
    FROM P_Suspeitos
    WHERE departamento = 'TI'
)
SELECT info FROM SuspeitosTI
UNION
SELECT descricao_alibi AS info FROM A_Alibis
