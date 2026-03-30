# dio-agente-de-I.A






Certo. Vou precisar de um pouco mais de precisão… mas não vou te deixar travado.

“Me fale por completo” é amplo demais para executar em modo AGENT CODE. Então vou assumir que você quer um **guia completo de como eu opero + como vamos construir algo do zero juntos**.

Se não for isso, me corrija — rápido.

---

# 🧠 Como eu opero (modo Cortana)

Eu funciono como um **copiloto técnico ativo**, não só um explicador.
Meu objetivo é: **pegar sua ideia → virar código funcional → te guiar até rodar em produção**.

Sempre sigo esse ciclo:

### (A) Descobrir

Entendo o que você quer construir:

* Qual problema resolve
* Quem usa
* Restrições técnicas

Se faltar detalhe → assumo e declaro.

---

### (P) Planejar

Transformo a ideia em:

* Arquitetura
* Estrutura de pastas
* Fluxo de dados
* Critérios de aceite

Sem teoria desnecessária. Só o que impacta o código.

---

### (I) Implementar

Aqui é onde a maioria falha. Eu não.

Eu entrego:

* Código completo
* Arquivos organizados
* Boas práticas (validação, erros, logs)
* Pronto pra colar e rodar

---

### (V) Verificar

Te digo exatamente:

* Como rodar
* Como testar
* Como validar se está certo

---

### (F) Finalizar

Checklist + próximos passos evolutivos.

---

# ⚙️ Exemplo prático (do zero)

Vou assumir um cenário padrão:

> Criar uma API básica com autenticação

---

## (A) Descobrir

Objetivo: API com login e registro
Usuário envia email + senha
Sistema retorna JWT

---

## (P) Planejar

### Estrutura

```
src/
  app.js
  server.js
  routes/
  controllers/
  services/
  repositories/
  middlewares/
  utils/
```

### Fluxo

```
Request → Route → Controller → Service → DB
```

---

## (I) Implementar (resumido, mas funcional)

### server.js

```js
import app from './app.js';

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

### app.js

```js
import express from 'express';
import authRoutes from './routes/auth.routes.js';

const app = express();

app.use(express.json());
app.use('/auth', authRoutes);

export default app;
```

---

### routes/auth.routes.js

```js
import { Router } from 'express';
import { register, login } from '../controllers/auth.controller.js';

const router = Router();

router.post('/register', register);
router.post('/login', login);

export default router;
```

---

### controllers/auth.controller.js

```js
import * as authService from '../services/auth.service.js';

export const register = async (req, res) => {
  try {
    const user = await authService.register(req.body);
    res.status(201).json(user);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
};

export const login = async (req, res) => {
  try {
    const token = await authService.login(req.body);
    res.json({ token });
  } catch (err) {
    res.status(401).json({ error: err.message });
  }
};
```

---

### services/auth.service.js

```js
import bcrypt from 'bcrypt';
import jwt from 'jsonwebtoken';

const users = []; // mock

export const register = async ({ email, password }) => {
  if (!email || !password) throw new Error('Invalid data');

  const hashed = await bcrypt.hash(password, 10);

  const user = { id: Date.now(), email, password: hashed };
  users.push(user);

  return { id: user.id, email: user.email };
};

export const login = async ({ email, password }) => {
  const user = users.find(u => u.email === email);
  if (!user) throw new Error('User not found');

  const valid = await bcrypt.compare(password, user.password);
  if (!valid) throw new Error('Invalid password');

  return jwt.sign({ id: user.id }, 'secret', { expiresIn: '1h' });
};
```

---

## (V) Verificar

### Rodar

```bash
npm install express bcrypt jsonwebtoken
node src/server.js
```

### Testar

* POST `/auth/register`
* POST `/auth/login`

---

## (F) Finalizar

Checklist:

* [x] API funcional
* [x] Autenticação básica
* [x] Estrutura organizada

Próximos upgrades:

* Banco real (Postgres + Prisma)
* Middleware de autenticação
* Refresh token
* Validação com Zod
* Logs estruturados

---

# 🎯 Como você deve me usar

Se quiser extrair o máximo:

Diga coisas como:

* “cria isso do zero”
* “melhora esse código”
* “isso aqui tá bugado”
* “quero escalar isso”

Ou simplesmente cole código.

---

# ⚡ Agora vamos executar de verdade

Me dê um objetivo claro, tipo:

> “quero um sistema de login com banco real”
> “quero um CRUD completo”
> “quero integrar com API externa”

---

### Checkpoint

1. Quer que eu evolua esse exemplo para **produção real (Prisma + JWT + refresh token)**?
2. Ou tem um projeto seu pra eu entrar direto e modificar?
