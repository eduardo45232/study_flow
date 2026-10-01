<div align="center">

# 📚 StudyFlow

**Sua plataforma de estudos organizada, focada no ENEM e nos conteúdos essenciais.**

Organize matérias, filtre por nível e pratique com questões reais do ENEM — tudo em um app Flutter rápido e moderno.

![Flutter](https://img.shields.io/badge/Flutter-3.12+-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![Dart](https://img.shields.io/badge/Dart-3.0+-0175C2?style=for-the-badge&logo=dart&logoColor=white)
![Material 3](https://img.shields.io/badge/Material_3-Yes-6750A4?style=for-the-badge&logo=material-design&logoColor=white)
![Platform](https://img.shields.io/badge/Android_iOS_Web_Windows-Linux_macOS-000000?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

[Funcionalidades](#-funcionalidades) • [Demo](#-como-funciona) • [Tecnologias](#-tecnologias) • [Instalação](#-instalação-e-execução) • [Estrutura](#-estrutura-do-projeto) • [API](#-integração-com-api-do-enem) • [Roadmap](#-roadmap)

</div>

---

## ✨ Sobre o projeto

O **StudyFlow** é um aplicativo Flutter para organização de estudos voltado ao **ENEM e vestibulares**.

Ele resolve um problema simples: conteúdo espalhado e sem progressão clara. Aqui você tem:

1. **Áreas de estudo** organizadas (Matemática, Linguagens, Ciências Humanas, Ciências da Natureza)
2. **Conteúdos classificados por nível** — Básico, Intermediário e Avançado
3. **Questões reais do ENEM** consumidas via API, com alternativas, imagens e correção automática

> Perfeito para estudantes que querem revisar teoria e praticar questões no mesmo lugar.

---

## 🚀 Funcionalidades

| Área | Descrição |
|------|-----------|
| 🏠 **Home / Áreas de estudo** | Lista de matérias com contagem de conteúdos e acesso rápido às questões do ENEM |
| 📖 **Página da Matéria** | Descrição + filtro por nível com `ChoiceChip` (Todos / Básico / Intermediário / Avançado) |
| 📝 **Página de Conteúdo** | Detalhe do conteúdo com título, nível e descrição |
| 🎯 **Questões do ENEM** | Lista de questões da API `api.enem.dev` com loading, erro + botão `Tentar novamente` e estado vazio |
| ✅ **Correção interativa** | Toque em uma alternativa para registrar a resposta e ver na hora se acertou + gabarito oficial |
| 🖼️ **Suporte a imagens** | Enunciados e alternativas com imagem, com fallback amigável se indisponível |
| 🎨 **Material 3** | Tema moderno com `ColorScheme.fromSeed` e navegação por `Navigator` |

### Fluxo do usuário

```
Home (Áreas) → Matéria (filtro por nível) → Conteúdo (detalhe)
             ↳ Questões ENEM (lista) → Questão (responder + gabarito)
```

---

## 🛠️ Tecnologias

**Core:**
- [Flutter](https://flutter.dev/) 3.12+ / Dart 3.12+
- Material 3 + `ColorScheme.fromSeed`
- Navegação imperativa (`Navigator + MaterialPageRoute`)

**Pacotes:**
- `http: ^1.6.0` — consumo da API do ENEM
- `cupertino_icons: ^1.0.8` — ícones iOS
- `flutter_lints: ^6.0.0` — padrão de lint (dev)

**API externa:**
- `https://api.enem.dev/v1/exams/{ano}/questions` — questões, alternativas, imagens e gabarito

---

## 📁 Estrutura do projeto

```bash
lib/
├── main.dart                  # Entry point + StudyFlowApp (MaterialApp + Theme)
├── data/
│   └── dados_estudo.dart      # Dados mockados: 4 matérias + conteúdos por nível
├── models/
│   ├── materia.dart           # Materia { nome, descricao, conteudos }
│   ├── conteudo.dart          # Conteudo { titulo, descricao, nivel }
│   └── questao_enem.dart      # QuestaoEnem + AlternativaEnem (fromJson)
├── service/
│   └── enem_service.dart      # EnemService.buscarQuestoes() via http
├── screens/
│   ├── home_page.dart         # Áreas de estudo + botão Questões do ENEM
│   ├── materia_page.dart      # Filtro por nível + lista de conteúdos
│   ├── conteudo_page.dart     # Detalhe do conteúdo
│   ├── enem_page.dart         # FutureBuilder: loading / erro / lista
│   └── questao_enem_page.dart # Enunciado + alternativas + correção
└── widgets/
    ├── materia_card.dart      # Card da matéria
    └── conteudo_card.dart     # Card do conteúdo
```

**Padrão aplicado:** UI em `screens/`, componentes reutilizáveis em `widgets/`, regra de rede isolada em `service/`, modelos imutáveis com `fromJson`.

---

## 🔌 Integração com API do ENEM

Arquivo: `lib/service/enem_service.dart:7`

```dart
GET https://api.enem.dev/v1/exams/2022/questions?limit=10&offset=0
// timeout: 15s
```

Resposta esperada:

```json
{
  "questions": [
    {
      "index": 1,
      "title": "Questão 1",
      "discipline": "mathematics",
      "context": "Enunciado...",
      "alternativasIntroduction": "...",
      "correctAlternative": "B",
      "files": ["https://.../imagem.png"],
      "alternatives": [
        { "letter": "A", "text": "...", "file": null }
      ]
    }
  ]
}
```

Tratamento de erros implementado:
- `statusCode != 200` → `Exception`
- JSON fora do padrão → `FormatException`
- UI reage com tela de erro + `Tentar novamente` (`lib/screens/enem_page.dart:33`)

---

## 💻 Instalação e execução

### Pré-requisitos

- Flutter SDK 3.12.2+ — [guia de instalação](https://docs.flutter.dev/get-started/install)
- Dart 3.12+
- Android Studio / VS Code + extensão Flutter
- Emulador ou dispositivo físico

### 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/study_flow.git
cd study_flow
```

### 2. Instale as dependências

```bash
flutter pub get
```

### 3. Verifique o ambiente

```bash
flutter doctor
```

### 4. Execute o app

```bash
# Debug (dispositivo conectado / emulador aberto)
flutter run

# Web
flutter run -d chrome

# Windows
flutter run -d windows
```

### 5. Build para produção

```bash
flutter build apk --release
flutter build appbundle --release
flutter build web --release
flutter build windows --release
```

### Testes e análise

```bash
flutter test
flutter analyze
```

---

## 📱 Como funciona

### 1. Tela inicial
Abra o app → veja as **Áreas de estudo** e toque em `Questões do ENEM` ou em uma matéria.

### 2. Estudar por nível
Dentro da matéria, use os filtros `Todos / Básico / Intermediário / Avançado` para progredir com consistência.

### 3. Praticar questões
Vá em **Questões do ENEM** → abra uma questão → toque em uma alternativa → receba feedback imediato:

- `Você acertou! Gabarito: B`
- `Você marcou A. Gabarito: B`
- `Gabarito indisponível` (quando a API não retorna)

---

## 🗺️ Roadmap

- [ ] Barra de progresso por matéria / conteúdo concluído
- [ ] Persistência local (shared_preferences / hive) para marcar como estudado
- [ ] Filtro de questões por ano e disciplina
- [ ] Modo escuro
- [ ] Busca global de conteúdos
- [ ] Estatísticas de acertos no ENEM
- [ ] Testes de widget para `EnemService` e filtros

Quer contribuir? Abra uma issue ou PR! Roadmap completo pode virar Projects no GitHub.

---

## 🤝 Contribuindo

1. Faça um fork
2. Crie uma branch: `git checkout -b feat/minha-feature`
3. Commit: `git commit -m "feat: minha feature"`
4. Push: `git push origin feat/minha-feature`
5. Abra um Pull Request

Padrões:
- `flutter analyze` sem erros
- `flutter_lints` respeitado
- Widgets pequenos em `widgets/`, telas em `screens/`

---

## 📄 Licença

Este projeto está sob a licença **MIT**. Veja o arquivo `LICENSE` para mais detalhes.

---

<div align="center">

**Feito com 💙 em Flutter para estudantes focados no ENEM.**

⭐ Se este projeto te ajudou, deixe uma estrela!

</div>
