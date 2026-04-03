# CONTEXTO TOTAL PARA ANTIGRAVITY: "PUERTA DUBAI NETWORK"

**OBJETIVO:** Transformar un ecosistema de Landings de servicios en una red de captación con atribución de por vida.

## 1. EL "DISPARADOR" (La URL)
Todas las landings deben estar preparadas para recibir un parámetro en la URL llamado `ref`.
* **Ejemplo:** `puertadubai.online/viajes?ref=ABC123`
* **Acción:** Al detectar `ref`, el sistema debe guardar ese valor en el **LocalStorage** del navegador (persistente) bajo la clave `p_dubai_affiliate`.

## 2. LA CAPA DE "RECONOCIMIENTO" (Script Global)
Cada página del sitio (Empresas, Viajes, Residencias, Marketing) debe tener un script que:
* Si el usuario navega entre páginas (de viajes a empresas), el código `ref` debe seguirlo.
* Si el usuario cierra la web y vuelve a los 10 días, el código debe seguir ahí (Atribución "First Click").

## 3. EL MOMENTO DE CAPTURA (Formularios y WhatsApp)
* **Formularios de Contacto:** Todos los formularios deben tener un **campo oculto (hidden field)** llamado `referido_id`. El script debe llenar este campo automáticamente con el valor guardado en LocalStorage antes de que el cliente le dé a "Enviar".
* **Botones de WhatsApp:** El enlace del botón debe modificarse dinámicamente para incluir el código.
* *Resultado:* Tu equipo recibe un lead que ya dice quién lo envió en la base de datos o en el chat.

## 4. LA EXPERIENCIA DEL EMBAJADOR (Dashboard)
Portal de Socios (`network.puertadubai.online`):
* **Pantalla de Registro:** Para nuevos embajadores (recolecta nombre, tel, email y quién lo invitó).
* **Pantalla de Login:** Acceso a su oficina virtual.
* **El Panel (Dashboard):** Visualización de Balances (USD, CUP, Bono Viaje).
* **Generador de Links:** Un botón por cada servicio que tome la URL base y le pegue el código del usuario.
* **Lista de Prospectos:** Ver sus leads registrados y el estado (Procesando / Ganado).

## 5. LÓGICA DE NEGOCIO (Reglas de Oro)
* **Identificador Único:** El Teléfono del cliente es la llave. Si ya existe, se bloquea el registro para otro embajador.
* **Validación Manual:** La app debe indicar que el saldo está "Pendiente" hasta que el Administrador valide el ingreso del dinero en Dubái.

---

# Plan de Compensación (Blindado)

* **Bono Viral (Captación):** 50 CUP por cada registro de nuevo embajador. (10 CUP en algunas fases como bono inicial para mover balance).
* **Cláusula de Desbloqueo:** El saldo de bonos solo se vuelve "Retirable" cuando ocurra una venta aprobada en su linaje (ya sea de él mismo o de alguien en sus 3 niveles).
* **Mínimo de Retiro:** 5,000 CUP (o $50 USD).
* **Comisiones por Utilidad (Dubái) - Modelo 15% de la Ganancia:**
  * Nivel 1 (Vendedor Directo): 10% de la utilidad ($50 - $80 USD).
  * Nivel 2 (Padre/Líder): 3% de la utilidad ($15 USD).
  * Nivel 3 (Abuelo/Mentor): 2% de la utilidad ($10 USD).
* **Fondo de Reserva (Bono Dubái):** 5% de retención sobre comisiones para la meta del viaje.

---

# Arquitectura del Sistema

## 1. Capa de Datos: El Corazón (Supabase)
Todo está en 4 tablas clave:
* `aff_users` (Embajadores): id, nombre, email, codigo_unico, parent_id, balance_usd, balance_cup, balance_bonos_cup, balance_comisiones_usd, red_activa (boolean), total_registros.
* `aff_clients` (Leads/Prospectos): id, nombre, telefono (UNIQUE), email, servicio_interesado, embajador_id, estado_comercial.
* `aff_sales` (Ventas Reales): id, cliente_id, monto_total, comprobante_url, estado_pago.
* `aff_commissions` (Contabilidad): id, venta_id, embajador_id, monto, nivel, tipo.

## 2. Capa de Usuario: Los Frontends (Google Stitch / Antigravity)
* **App del Embajador:** Dashboard, Arsenal de Ventas, Mis Referidos, Retiros.
* **App del Administrador:** Validación de Ventas, Gestión de Red, Control de Pagos.

## 3. Capa de Captura: Tus Landings (Antigravity)
* Script Maestro guarda el código en el navegador.
* Formularios inyectan el código del embajador de forma invisible.

## 4. Capa de Inteligencia: Automatizaciones (n8n)
* **Flujo 1 (Nuevo Lead):** Notifica en Telegram/WhatsApp.
* **Flujo 2 (Bono Viral):** Suma CUP y envía WhatsApp al "padre".
* **Flujo 3 (Aprobación):** Calcula comisiones (10/3/2 o 10/4/2), cambia `red_activa` a TRUE, y notifica pagos.
