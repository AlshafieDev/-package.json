 -- 1. إنشاء الجداول
CREATE TABLE traders (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    phone VARCHAR(20) UNIQUE NOT NULL,
    whatsapp VARCHAR(20),
    region VARCHAR(50) NOT NULL,
    city VARCHAR(50),
    is_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE gold_prices (
    id SERIAL PRIMARY KEY,
    region VARCHAR(50) NOT NULL,
    karat INTEGER NOT NULL,
    buy_price BIGINT NOT NULL,
    sell_price BIGINT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- 2. تفعيل الحماية (RLS) - هذا أهم سطر في مشروعك!
ALTER TABLE traders ENABLE ROW LEVEL SECURITY;
ALTER TABLE gold_prices ENABLE ROW LEVEL SECURITY;

-- 3. إعداد السياسات (الجميع يقرأ، لكن لا أحد يعدل إلا أنت)
CREATE POLICY "Allow public read access to prices" ON gold_prices FOR SELECT USING (true);
CREATE POLICY "Allow public read access to verified traders" ON traders FOR SELECT USING (is_verified = TRUE);

-- 4. إدخال بيانات تجريبية (لتجربة التطبيق)
INSERT INTO gold_prices (region, karat, buy_price, sell_price) VALUES 
('الخرطوم', 21, 115000, 117000),
('دار مالي', 21, 112000, 114000),
('أبو حمد', 24, 130000, 133000);
