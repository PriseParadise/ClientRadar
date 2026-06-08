ClientRadar
Herramienta comercial para análisis de ventas, preparación de visitas y seguimiento de clientes en herbolarios y farmacias.

¿Qué hace?

Dashboard de ventas con KPIs, evolución mensual y comparativas por año
Alertas comerciales — clientes desaparecidos, en riesgo, referencias caídas y oportunidades
Ficha de cliente — historial completo, comparativa de periodos y top productos
Preparación de visita — resumen listo para llevar a la reunión
Informes — exporta PDF por cliente o Excel con resumen, comparativa y alertas
Multi-usuario — cada comercial tiene sus propios datos en la nube


Tecnología

HTML + CSS + JavaScript (sin frameworks, un solo fichero)
Chart.js para los gráficos
SheetJS para leer y exportar Excel
Supabase como base de datos en la nube
Desplegado en Vercel


Estructura del proyecto
/
└── index.html    ← toda la aplicación en un solo fichero
└── README.md

Cómo importar datos
La app acepta dos formatos de Excel:
Formato estándar — columnas requeridas:
mesañoclienteproductounidadesimporteenero2024Farmacia XProducto Y10150.00
Formato Navision / ERP — la app detecta y limpia automáticamente la estructura exportada desde Navision. Solo hay que indicar el mes y año del fichero.

Configuración de Supabase (para desarrolladores)
1. Crear las tablas
En SQL Editor de Supabase:
sqlCREATE TABLE users (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  name text NOT NULL,
  color text DEFAULT '#4f7cff',
  created_at timestamp DEFAULT now()
);

CREATE TABLE sales_data (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id uuid REFERENCES users(id) ON DELETE CASCADE,
  data jsonb NOT NULL,
  uploaded_at timestamp DEFAULT now()
);

CREATE TABLE manual_months (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id uuid REFERENCES users(id) ON DELETE CASCADE,
  month_data jsonb NOT NULL,
  created_at timestamp DEFAULT now()
);
2. Activar permisos de acceso
sqlALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE sales_data ENABLE ROW LEVEL SECURITY;
ALTER TABLE manual_months ENABLE ROW LEVEL SECURITY;

CREATE POLICY "public_all" ON users FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "public_all" ON sales_data FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "public_all" ON manual_months FOR ALL USING (true) WITH CHECK (true);
3. Actualizar las claves en index.html
Al principio del bloque <script> en index.html, reemplaza:
jsconst SUPA_URL = 'https://TU_PROYECTO.supabase.co';
const SUPA_KEY = 'TU_ANON_KEY';

Despliegue en Vercel

Sube index.html a un repositorio de GitHub
Conecta el repo en vercel.com
Vercel desplegará automáticamente con cada push a main


Uso básico

Abre la app y crea tu usuario con tu nombre
Ve a Importar Excel y sube tu fichero de ventas
Explora el Dashboard, las Alertas y la Ficha de Cliente
Usa Preparación de Visita antes de ir a ver a un cliente
Exporta Informes en PDF o Excel cuando los necesites


Notas

Los datos se guardan en la nube (Supabase) — funcionan en cualquier dispositivo y navegador
Cada usuario ve solo sus propios datos
No se requiere contraseña — el acceso es por nombre de usuario
