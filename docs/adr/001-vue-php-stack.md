# ADR 001: Escolha da Stack TecnolÃ³gica Vue.js + PHP

**Status**: Aceito  
**Data**: 2024-01-15  
**Decisores**: Equipe de Desenvolvimento DTI

## Contexto

O Portal de InclusÃ£o precisava de uma stack tecnolÃ³gica que atendesse aos seguintes requisitos:
- Interface moderna e responsiva
- Desenvolvimento rÃ¡pido com recursos limitados
- Compatibilidade com infraestrutura existente (XAMPP/Apache)
- Facilidade de manutenÃ§Ã£o pela equipe municipal

## DecisÃ£o

Adotamos **Vue.js 3** para o frontend e **PHP 8+** para o backend.

### Frontend: Vue.js 3
- Framework progressivo com curva de aprendizado suave
- Composition API para melhor organizaÃ§Ã£o de cÃ³digo
- Ecossistema rico (Vue Router, Vite)
- Excelente documentaÃ§Ã£o em portuguÃªs

### Backend: PHP 8+
- JÃ¡ utilizado em outros sistemas municipais
- FÃ¡cil deploy em servidores Apache/XAMPP
- PDO nativo para acesso a banco de dados
- Sem necessidade de frameworks pesados

## Alternativas Consideradas

### 1. React + Node.js
**PrÃ³s:**
- Ecossistema maior
- Melhor para aplicaÃ§Ãµes complexas

**Contras:**
- Curva de aprendizado mais Ã­ngreme
- Requer Node.js em produÃ§Ã£o
- Mais complexo para equipe pequena

### 2. Laravel + Blade
**PrÃ³s:**
- Framework PHP completo
- ConvenÃ§Ãµes estabelecidas

**Contras:**
- Overhead desnecessÃ¡rio para API simples
- Blade nÃ£o oferece reatividade moderna
- Mais pesado que soluÃ§Ã£o atual

### 3. WordPress + Plugins
**PrÃ³s:**
- Familiaridade da equipe
- Admin pronto

**Contras:**
- NÃ£o adequado para aplicaÃ§Ã£o customizada
- Performance inferior
- SeguranÃ§a questionÃ¡vel

## ConsequÃªncias

### Positivas
âœ… Desenvolvimento rÃ¡pido e iterativo  
âœ… FÃ¡cil onboarding de novos desenvolvedores  
âœ… Deploy simples em infraestrutura existente  
âœ… SeparaÃ§Ã£o clara frontend/backend  
âœ… Possibilidade de migrar backend futuramente sem afetar frontend  

### Negativas
âš ï¸ PHP nÃ£o Ã© ideal para aplicaÃ§Ãµes real-time  
âš ï¸ Escalabilidade limitada comparada a Node.js  
âš ï¸ Sem tipagem estÃ¡tica no backend (mitigado com PHPStan)  

### Neutras
- Necessidade de dois servidores em desenvolvimento (Vite + PHP)
- AutenticaÃ§Ã£o via token em localStorage (padrÃ£o para SPAs)

## Notas de ImplementaÃ§Ã£o

- **Vite** escolhido como build tool (mais rÃ¡pido que Webpack)
- **TailwindCSS** para estilizaÃ§Ã£o (produtividade)
- **PDO** para banco de dados (seguranÃ§a contra SQL injection)
- Servidor PHP built-in para desenvolvimento (`php -S`)

## ReferÃªncias

- [Vue.js Documentation](https://vuejs.org/)
- [PHP: The Right Way](https://phptherightway.com/)
- [VisÃ£o Geral da Arquitetura](../architecture/overview.md)

