# ADR 001: Escolha da Stack Tecnológica Vue.js + PHP

**Status**: Aceito  
**Data**: 2024-01-15  
**Decisores**: Equipe de Desenvolvimento DTI

## Contexto

O Portal de Inclusão precisava de uma stack tecnológica que atendesse aos seguintes requisitos:
- Interface moderna e responsiva
- Desenvolvimento rápido com recursos limitados
- Compatibilidade com infraestrutura existente (XAMPP/Apache)
- Facilidade de manutenção pela equipe municipal

## Decisão

Adotamos **Vue.js 3** para o frontend e **PHP 8+** para o backend.

### Frontend: Vue.js 3
- Framework progressivo com curva de aprendizado suave
- Composition API para melhor organização de código
- Ecossistema rico (Vue Router, Vite)
- Excelente documentação em português

### Backend: PHP 8+
- Já utilizado em outros sistemas municipais
- Fácil deploy em servidores Apache/XAMPP
- PDO nativo para acesso a banco de dados
- Sem necessidade de frameworks pesados

## Alternativas Consideradas

### 1. React + Node.js
**Prós:**
- Ecossistema maior
- Melhor para aplicações complexas

**Contras:**
- Curva de aprendizado mais íngreme
- Requer Node.js em produção
- Mais complexo para equipe pequena

### 2. Laravel + Blade
**Prós:**
- Framework PHP completo
- Convenções estabelecidas

**Contras:**
- Overhead desnecessário para API simples
- Blade não oferece reatividade moderna
- Mais pesado que solução atual

### 3. WordPress + Plugins
**Prós:**
- Familiaridade da equipe
- Admin pronto

**Contras:**
- Não adequado para aplicação customizada
- Performance inferior
- Segurança questionável

## Consequências

### Positivas
✅ Desenvolvimento rápido e iterativo  
✅ Fácil onboarding de novos desenvolvedores  
✅ Deploy simples em infraestrutura existente  
✅ Separação clara frontend/backend  
✅ Possibilidade de migrar backend futuramente sem afetar frontend  

### Negativas
⚠️ PHP não é ideal para aplicações real-time  
⚠️ Escalabilidade limitada comparada a Node.js  
⚠️ Sem tipagem estática no backend (mitigado com PHPStan)  

### Neutras
- Necessidade de dois servidores em desenvolvimento (Vite + PHP)
- Autenticação via token em localStorage (padrão para SPAs)

## Notas de Implementação

- **Vite** escolhido como build tool (mais rápido que Webpack)
- **TailwindCSS** para estilização (produtividade)
- **PDO** para banco de dados (segurança contra SQL injection)
- Servidor PHP built-in para desenvolvimento (`php -S`)

## Referências

- [Vue.js Documentation](https://vuejs.org/)
- [PHP: The Right Way](https://phptherightway.com/)
- [Visão Geral da Arquitetura](../architecture/overview.md)
