# YugabyteDB-User-Management
When managing user roles and permissions in YugabyteDB for a Git project, you might want to consider the following aspects of user management.

1. User Creation and Management
Create users with specific roles that align with your project's requirements.
Use SQL commands such as CREATE USER and ALTER USER to manage user properties.

2. Role-Based Access Control (RBAC)
Define roles that encapsulate permissions for various operations (e.g., read, write, admin).
Assign these roles to users to control access to different database resources.

3. Granting Permissions
Use the GRANT statement to assign specific privileges to users or roles.
This can include permissions on tables, databases, or schemas based on the needs of your Git project.

4. Revoking Permissions
Use the REVOKE statement to remove permissions from users or roles when they no longer need access.

5. Auditing and Monitoring
Keep track of user activities and changes in permissions for security and compliance purposes.
Implement logging mechanisms to monitor access patterns.

---Create database:

CREATE DATABASE myshop;



---Create table:

CREATE TABLE products (

    product\_id SERIAL PRIMARY KEY,

    name TEXT NOT NULL,

    price NUMERIC(10,2) NOT NULL CHECK (price >= 0),

    available BOOLEAN DEFAULT true,

    created\_at TIMESTAMPTZ DEFAULT now()

);



---Sample inserts:
INSERT INTO products (name, price) VALUES

('Apple iPhone 14', 799.99),

('Samsung Galaxy S23', 749.49),

('OnePlus 11', 699.00),

('Sony WH-1000XM5 Headphones', 349.99),

('Apple MacBook Air M2', 1199.00),

('Dell XPS 13', 999.99),

('Logitech MX Master 3S Mouse', 99.99),

('HP 24-inch Monitor', 179.49),

('Canon EOS 1500D Camera', 450.00),

('Kindle Paperwhite', 129.99);





-- Create user:
CREATE USER myshop\_user WITH PASSWORD 'mypassword';

CREATE ROLE shop\_user WITH LOGIN PASSWORD 'StrongP@ss123';



-- Grant privileges:

GRANT CONNECT ON DATABASE myshop TO shop\_user;

GRANT USAGE ON SCHEMA public TO myshop\_user;       -- Access to objects in the schema

GRANT SELECT ON ALL TABLES IN SCHEMA public TO myshop\_user;  -- Read data from all tables

ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO myshop\_user;  --To automatically grant SELECT on future tables too:

-- Grant privileges to database:
GRANT CONNECT ON DATABASE myshop TO myshop\_user;

-- Grant privileges to particular table:

GRANT SELECT ON TABLE public.products TO myshop\_user;

---grant table permissions

GRANT ALL PRIVILEGES ON TABLE products TO shop\_user;

--Revoke access

REVOKE SELECT ON TABLE public.products FROM myshop\_user; --Table level access

REVOKE USAGE ON SCHEMA public FROM myshop\_user; --Schema level access

REVOKE CONNECT ON DATABASE myshop FROM myshop\_user; --database level access

REVOKE ALL PRIVILEGES ON TABLE public.products FROM myshop\_user; -- All privileges



