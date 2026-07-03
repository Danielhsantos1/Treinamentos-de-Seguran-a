[README.md](https://github.com/user-attachments/files/29619667/README.md)
# 🦺 SafeTrack – Gestão de Treinamentos de Segurança

Sistema web completo para gestão de treinamentos de segurança do trabalho, certificados, lista de presença e QR Code de colaboradores.

---

## ✅ Funcionalidades

- 🔐 **Login por e-mail e senha** (Firebase Auth)
- 👑 **Perfil Admin** com visão total de todos os dados
- 🏢 **Cadastro de Empresas** com CNPJ com máscara e validação
- 👷 **Cadastro de Alunos** vinculados à empresa com CPF validado
- 🏅 **Emissão de Certificados** com vencimento calculado automaticamente
- 🔴🟡🟢 **Semáforo de status** (Válido / A vencer / Vencido)
- ✅ **Lista de presença** por turma e data
- 📱 **Gerador de QR Code** para capacete, crachá ou uniforme
- 📷 **Scanner QR Code real** com câmera do celular
- ⬇️ **Certificado em PDF** gerado diretamente no app
- 🔗 **Link compartilhável** do certificado
- 🔒 **Multi-usuário** — cada escola vê apenas seus próprios dados
- 📲 **100% mobile** — funciona no celular via navegador

---

## 🔧 Como configurar o Firebase

### 1. Criar projeto
1. Acesse [firebase.google.com](https://firebase.google.com)
2. Crie um projeto chamado `SafeTrack`
3. Clique em **`</>`** para adicionar app Web
4. Copie o objeto `firebaseConfig`

### 2. Substituir credenciais
No arquivo `index.html`, localize e substitua:

```js
const firebaseConfig = {
  apiKey: "SUA_API_KEY",
  authDomain: "SEU_PROJETO.firebaseapp.com",
  projectId: "SEU_PROJETO",
  storageBucket: "SEU_PROJETO.firebasestorage.app",
  messagingSenderId: "SEU_ID",
  appId: "SEU_APP_ID"
};
```

### 3. Ativar Authentication
1. No Firebase Console → **Authentication → Sign-in method**
2. Ative **E-mail/Senha**

### 4. Criar Firestore Database
1. **Firestore Database → Criar banco de dados**
2. Selecione **Edição Standard**
3. Localização: **southamerica-east1 (São Paulo)**
4. Inicie no **modo de teste** (depois configure as regras abaixo)

### 5. Regras de segurança do Firestore

Acesse **Firestore → Regras** e cole:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{collection}/{document} {
      allow read, write: if request.auth != null
        && (request.auth.token.email == "admin@safetrack.com.br"
            || resource.data.userId == request.auth.uid
            || request.resource.data.userId == request.auth.uid);
    }
  }
}
```

---

## 👑 Configurar Admin

No arquivo `index.html`, localize e altere o e-mail admin:

```js
window.ADMIN_EMAIL = "admin@safetrack.com.br";
```

O admin:
- Vê **todos os dados** de todos os usuários
- Pode excluir qualquer registro
- É identificado com badge 👑 ADMIN no app

---

## 🚀 Como publicar (Netlify — gratuito)

1. Acesse [netlify.com](https://netlify.com)
2. Faça login com Google
3. Arraste o arquivo `index.html` para a área de deploy
4. Em 30 segundos você recebe um link público
5. ⚠️ O scanner de QR Code **exige HTTPS** — o Netlify já fornece automaticamente

---

## 🌐 Como publicar (Firebase Hosting — gratuito)

```bash
# Instalar Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Iniciar projeto
firebase init hosting

# Selecionar seu projeto SafeTrack
# Pasta pública: . (ponto)
# Single page app: Não

# Publicar
firebase deploy
```

---

## 📁 Estrutura do repositório

```
safetrack/
├── index.html      ← App completo (único arquivo)
└── README.md       ← Este arquivo
```

---

## 📱 Como usar o QR Code

1. Cadastre o aluno no app
2. Vá em **QR Code → Gerar**
3. Selecione o aluno e clique em **Baixar QR Code PNG**
4. Imprima em adesivo vinil resistente
5. Cole no capacete, crachá ou uniforme
6. Qualquer pessoa com o app pode escanear em **QR Code → Escanear** e ver todos os certificados

---

## 🔒 Segurança

- Todos os dados têm `userId` do usuário autenticado
- Usuários normais veem **apenas seus próprios dados**
- Admin definido por e-mail no código vê tudo
- Sem login = sem acesso

---

## 📞 Suporte

Sistema desenvolvido com Firebase + HTML/CSS/JS puro.
Sem dependências de build. Funciona direto no navegador.
