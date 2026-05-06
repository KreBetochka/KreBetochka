import { useEffect, useState } from "react";

/* =========================
   ТИПЫ
========================= */

type Metric = {
  users: number;
  revenue: number;
  requests: number;
};

type Project = {
  id: number;
  title: string;
  description: string;
  stack: string[];
};

/* =========================
   МОДЕЛЬ API (эмуляция backend)
========================= */

const api = {
  async getMetrics(): Promise<Metric> {
    return new Promise((res) =>
      setTimeout(
        () =>
          res({
            users: 18240,
            revenue: 640000,
            requests: 1200340,
          }),
        700
      )
    );
  },

  async getProjects(): Promise<Project[]> {
    return new Promise((res) =>
      setTimeout(
        () =>
          res([
            {
              id: 1,
              title: "TaskFlow SaaS",
              description:
                "Масштабируемая система управления задачами с авторизацией и API.",
              stack: ["React", "TypeScript", "Zustand", "REST API"],
            },
            {
              id: 2,
              title: "Крипто-аналитика",
              description:
                "Дашборд с графиками и интеграцией внешних API в реальном времени.",
              stack: ["React", "Chart.js", "API", "TypeScript"],
            },
            {
              id: 3,
              title: "E-commerce интерфейс",
              description:
                "Масштабируемый интерфейс интернет-магазина с корзиной.",
              stack: ["React", "Redux", "TypeScript"],
            },
          ]),
        600
      )
    );
  },
};

/* =========================
   ПРИЛОЖЕНИЕ
========================= */

export default function ПортфолиоFrontend() {
  const [метрики, setМетрики] = useState<Metric | null>(null);
  const [проекты, setПроекты] = useState<Project[]>([]);
  const [загрузка, setЗагрузка] = useState(true);
  const [сообщение, setСообщение] = useState("");

  /* =========================
     ЗАГРУЗКА ДАННЫХ
  ========================= */

  useEffect(() => {
    async function загрузить() {
      setЗагрузка(true);

      const [m, p] = await Promise.all([
        api.getMetrics(),
        api.getProjects(),
      ]);

      setМетрики(m);
      setПроекты(p);
      setЗагрузка(false);
    }

    загрузить();
  }, []);

  /* =========================
     ФОРМА КОНТАКТА
  ========================= */

  function отправить(e: React.FormEvent) {
    e.preventDefault();

    if (!сообщение) return alert("Введите сообщение");

    alert("Сообщение отправлено!");
    setСообщение("");
  }

  /* =========================
     UI
  ========================= */

  return (
    <div style={styles.app}>
      
      {/* НАВИГАЦИЯ */}
      <header style={styles.nav}>
        <h2>Портфолио Frontend-разработчика</h2>

        <nav style={styles.links}>
          <a href="#метрики">Метрики</a>
          <a href="#проекты">Проекты</a>
          <a href="#контакты">Контакты</a>
        </nav>
      </header>

      {/* ГЛАВНЫЙ ЭКРАН */}
      <section style={styles.hero}>
        <h1>Frontend разработчик (React / TypeScript)</h1>

        <p>
          Разрабатываю масштабируемые фронтенд-системы, архитектуру
          и высокопроизводительные веб-приложения.
        </p>

        <div style={styles.socials}>
          <a href="https://github.com/yourname">GitHub</a>
          <a href="https://linkedin.com/in/yourname">LinkedIn</a>
          <a href="https://t.me/yourname">Telegram</a>
        </div>
      </section>

      {/* МЕТРИКИ */}
      <section id="метрики" style={styles.section}>
        <h2>Системные метрики</h2>

        {загрузка && <p>Загрузка данных...</p>}

        {метрики && (
          <div style={styles.grid}>
            <div style={styles.card}>
              <h3>Пользователи</h3>
              <p>{метрики.users}</p>
            </div>

            <div style={styles.card}>
              <h3>Доход</h3>
              <p>${метрики.revenue}</p>
            </div>

            <div style={styles.card}>
              <h3>Запросы</h3>
              <p>{метрики.requests}</p>
            </div>
          </div>
        )}
      </section>

      {/* ПРОЕКТЫ */}
      <section id="проекты" style={styles.section}>
        <h2>Архитектурные проекты</h2>

        {загрузка ? (
          <p>Загрузка проектов...</p>
        ) : (
          <div style={styles.grid}>
            {проекты.map((p) => (
              <div key={p.id} style={styles.card}>
                <h3>{p.title}</h3>
                <p>{p.description}</p>

                <div style={styles.stack}>
                  {p.stack.map((t) => (
                    <span key={t} style={styles.tag}>
                      {t}
                    </span>
                  ))}
                </div>
              </div>
            ))}
          </div>
        )}
      </section>

      {/* ПОДХОД */}
      <section style={styles.section}>
        <h2>Инженерный подход</h2>

        <ul>
          <li>Масштабируемая архитектура фронтенда</li>
          <li>Переиспользуемые компоненты</li>
          <li>Оптимизация производительности</li>
          <li>UI на основе API</li>
        </ul>
      </section>

      {/* КОНТАКТ */}
      <section id="контакты" style={styles.section}>
        <h2>Контакты</h2>

        <form onSubmit={отправить} style={styles.form}>
          <textarea
            value={сообщение}
            onChange={(e) => setСообщение(e.target.value)}
            placeholder="Введите сообщение..."
            style={styles.textarea}
          />

          <button type="submit" style={styles.button}>
            Отправить
          </button>
        </form>

        <p>Email: frontend.dev@example.com</p>
      </section>

      {/* ФУТЕР */}
      <footer style={styles.footer}>
        <p>© 2026 Портфолио Frontend-разработчика</p>
      </footer>
    </div>
  );
}

/* =========================
   СТИЛИ
========================= */

const styles: Record<string, React.CSSProperties> = {
  app: {
    fontFamily: "Arial",
    background: "#0f0f0f",
    color: "#fff",
    minHeight: "100vh",
  },

  nav: {
    display: "flex",
    justifyContent: "space-between",
    padding: "20px",
    borderBottom: "1px solid #333",
  },

  links: {
    display: "flex",
    gap: "15px",
  },

  hero: {
    padding: "80px",
    textAlign: "center",
  },

  socials: {
    marginTop: "15px",
    display: "flex",
    justifyContent: "center",
    gap: "15px",
  },

  section: {
    padding: "50px",
  },

  grid: {
    display: "grid",
    gridTemplateColumns: "repeat(3, 1fr)",
    gap: "20px",
  },

  card: {
    border: "1px solid #333",
    padding: "20px",
    borderRadius: "10px",
  },

  stack: {
    marginTop: "10px",
    display: "flex",
    gap: "5px",
    flexWrap: "wrap",
  },

  tag: {
    border: "1px solid #555",
    padding: "4px 8px",
    fontSize: "12px",
  },

  form: {
    display: "flex",
    flexDirection: "column",
    gap: "10px",
    maxWidth: "400px",
  },

  textarea: {
    height: "100px",
    padding: "10px",
  },

  button: {
    padding: "10px",
    cursor: "pointer",
  },

  footer: {
    textAlign: "center",
    padding: "30px",
    borderTop: "1px solid #333",
  },
};