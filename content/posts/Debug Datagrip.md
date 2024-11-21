+++
title = 'Debug stored procedure using datagrid'
date = 2024-11-21T01:43:35+08:00
draft = false
+++
# 範例 PL/pgSQL

```postgresql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price NUMERIC(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE OR REPLACE FUNCTION add_product(
    p_name VARCHAR,
    p_description TEXT,
    p_price NUMERIC
)
    RETURNS INTEGER AS $$
DECLARE
    new_product_id INTEGER;
BEGIN
    -- 插入產品資料
    INSERT INTO products (name, description, price)
    VALUES (p_name, p_description, p_price)
    RETURNING id INTO new_product_id;

    -- 回傳新增的產品 ID
    RETURN new_product_id;
END;
$$ LANGUAGE plpgsql;

```

# 帶值傳入

stored procedure 通常都會放在 routines 的資料夾下

![](/images/2024-11-21_1.32.33.png)

右鍵點選要執行的檔案，選擇 run function，就會看到以下畫面，將值帶入後，記得要勾選左下角的 Run from console，接著按 OK

![](/images/2024-11-21_1.39.00.png)

就完成啦！

```
+-----------+--------------------------+
|id         |1                         |
+-----------+--------------------------+
|name       |鬆餅漢堡                   |
+-----------+--------------------------+
|description|買不到                     |
+-----------+--------------------------+
|price      |87.00                     |
+-----------+--------------------------+
|created_at |2024-11-20 17:20:29.818221|
+-----------+--------------------------+

```