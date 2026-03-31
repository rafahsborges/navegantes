# Navegantes

Estruturar um portal de agendamento e pagamentos com divisão automática de valores (split) exige uma arquitetura que integre gestão de usuários, catálogo de serviços e um processador de pagamentos robusto.

Para o seu cenário, o foco não é apenas a venda, mas a **automação do fluxo financeiro** e a conformidade fiscal.

---

## 1. O Motor de Pagamentos: Escolhendo a Ferramenta
Para fazer o que você descreveu (comissão estilo Hotmart), você precisa de um sistema com funcionalidade de **Marketplace Split**.

### Stripe (Recomendado)
É a solução mais completa para o que você busca. O produto específico chama-se **Stripe Connect**.
* **Como funciona:** Você (a plataforma) cria "Standard" ou "Express" accounts para cada psicólogo. 
* **O Split:** Quando o paciente paga R$ 200,00, o Stripe divide no ato: R$ 140,00 vão para a conta do psicólogo e R$ 60,00 ficam na sua conta de plataforma.
* **Câmbio:** Ele lida com conversão de moedas (Global Sales) de forma nativa, permitindo que o paciente pague em Libras e você receba em Reais.

### Alternativas Nacionais
Se o foco for 100% Brasil, o **Iugu** ou **Asaas** possuem APIs de split muito sólidas e taxas de boleto/Pix geralmente menores que as do Stripe, mas perdem na facilidade de conversão internacional.

---

## 2. Arquitetura do Sistema
Você precisará de três frentes de desenvolvimento:

### A. Dashboard do Psicólogo (Vendedor)
* Cadastro de perfil e especialidades.
* Configuração da conta bancária (vinculada ao Stripe Connect).
* Gestão de agenda e sessões (os "produtos").

### B. Portal do Paciente (Cliente)
* Busca por profissionais.
* Checkout transparente (pagamento sem sair do site).
* Histórico de sessões.

### C. Backend (Regras de Negócio)
* Aqui é onde você define a regra de **30% de comissão**. O sistema deve enviar para a API de pagamento a instrução: `amount: 1780, transfer_data: {destination: 'id_psicologo', amount: 1246}`.

---

## 3. A Questão Fiscal: O "Nó" do Faturamento
O exemplo que você deu sobre declarar 100% vs. 30% é o ponto mais crítico. No Brasil, o modelo que você deseja seguir chama-se **Conta Transpessoal** ou **Intermediação de Negócios**.

### O Cenário Ideal (Split de Notas Fiscais)
Para pagar imposto apenas sobre os seus 30%, a estrutura correta é:
1.  **O Psicólogo** emite uma nota fiscal de serviço para o **Paciente** (valor total da sessão).
2.  **Sua Plataforma** emite uma nota fiscal de serviço para o **Psicólogo** (referente aos 30% da taxa de intermediação).

**Por que isso é importante?**
Se você receber os 100% na sua conta bancária e depois transferir para o psicólogo, o fisco entenderá que os R$ 1.780 são **receita bruta sua**, e você será tributado sobre o todo. Com o **Stripe Connect**, o dinheiro tecnicamente "passa" pela plataforma mas é destinado diretamente aos envolvidos, o que facilita a defesa jurídica de que você é apenas o mediador.

---

## 4. Roteiro de Desenvolvimento (Stack Sugerida)
Considerando que você busca algo escalável e moderno:

* **Frontend:** Next.js (para boa indexação no Google e velocidade).
* **Backend:** Node.js (NestJS) — excelente para lidar com APIs financeiras e fluxos assíncronos.
* **Banco de Dados:** PostgreSQL (para transações financeiras, a consistência é prioridade).
* **Infraestrutura:** Docker para os ambientes e GitLab CI/CD para automatizar os deploys.

### Próximos Passos:
1.  **Validação Jurídica:** Consulte um contador especializado em infoprodutos/marketplaces para redigir os "Termos de Uso". É esse documento que define que você é um intermediador.
2.  **MVP (Produto Mínimo Viável):** Comece integrando o Stripe Checkout com uma regra fixa de split antes de criar dashboards complexos.
3.  **Automação de NF-e:** Considere usar ferramentas como **e-Notas** ou **PlugNotas** integradas ao seu sistema para que, assim que o pagamento for aprovado, as duas notas (do psicólogo e a sua) sejam geradas automaticamente.

### Explicação Técnica do Fluxo Proposto
1. Vendedores (Psicólogos/Consultores): Cada um tem um "ID de Destino" no Stripe Connect. O sistema lista os serviços deles como itens de um catálogo.
2. Checkout Transparente: Quando o paciente (ex: na Inglaterra) clica em comprar, o Stripe detecta a moeda local e aplica a conversão.
3. O Split Automático: O código JS acima demonstra o que acontece no backend: o valor total é quebrado. A Navegantes nunca "segura" o dinheiro do psicólogo; o Stripe faz a custódia e distribui as partes.
4. Fiscal: Como o sistema já separa as faturas, você consegue configurar para que o psicólogo receba o recibo do valor líquido e a plataforma emita a nota dos 30% sobre o serviço prestado ao profissional.

# ⚓ Navegantes: Propostas de Conceitos para Layout

Este documento detalha as cinco direções criativas propostas para o portal **Navegantes**, equilibrando a funcionalidade de marketplace de serviços (psicologia e consultoria) com a identidade náutica da marca.

---

## 1. O Porto Seguro
**Foco:** Confiança e Acolhimento.
* **Estilo Visual:** Minimalista, com uso predominante de espaços em branco, tons de azul marinho profundo e detalhes em cor de areia.
* **Elementos Gráficos:** Linhas finas, ícones de âncoras estilizadas e fotografias de portos calmos ao amanhecer.
* **Experiência do Usuário (UX):** Navegação extremamente clara e direta. Transmite a sensação de que o usuário "chegou ao destino" e está em boas mãos. Ideal para o público que busca suporte emocional (terapia).

## 2. Em Alto Mar
**Foco:** Expansão e Jornada.
* **Estilo Visual:** Imersivo. Uso de *Hero Backgrounds* (vídeos ou fotos em tela cheia) de veleiros cortando o oceano. Gradientes que lembram o azul do mar profundo.
* **Elementos Gráficos:** Botões flutuantes, tipografia moderna em negrito e transições suaves entre as seções.
* **Experiência do Usuário (UX):** Focada na narrativa (*storytelling*). Perfeito para vender a ideia de mudança de vida e consultoria migratória, inspirando coragem no usuário.

## 3. Bússola Estratégica
**Foco:** Precisão e Organização.
* **Estilo Visual:** Design em grid (quadrícula), muito organizado, com tipografia serifada clássica que remete a documentos antigos e mapas.
* **Elementos Gráficos:** Ícones de bússolas, rosas dos ventos e divisores de seção que lembram coordenadas geográficas.
* **Experiência do Usuário (UX):** Funcional e técnica. Facilita a comparação entre profissionais e a visualização clara de taxas, comissões e horários disponíveis.

## 4. Farol do Imigrante
**Foco:** Clareza e Direcionamento.
* **Estilo Visual:** *Dark Mode* elegante com contrastes em dourado ou ciano neon. O design utiliza luz e sombra para guiar o olhar.
* **Elementos Gráficos:** Feixes de luz virtuais que apontam para os botões de ação (CTAs) e ilustrações em *outline* (contorno).
* **Experiência do Usuário (UX):** Reduz o ruído visual para quem se sente "perdido" no exterior. O layout atua como um guia, destacando sempre o próximo passo do fluxo de contratação.

## 5. Linha do Horizonte
**Foco:** Conexão e Continuidade.
* **Estilo Visual:** Design horizontal e plano (*Flat Design*). Uso de *Parallax Scrolling* onde o horizonte parece se mover conforme o usuário desce a página.
* **Elementos Gráficos:** Ilustrações que conectam o ponto A (Brasil) ao ponto B (Exterior) por meio de uma linha contínua.
* **Experiência do Usuário (UX):** Enfatiza a ponte entre culturas. É o layout mais amigável e acessível, ideal para quem busca serviços de consultoria prática e rápida.

---

## Resumo Comparativo

| Conceito | Atributo Principal | Público-Alvo Sugerido |
| :--- | :--- | :--- |
| **Porto Seguro** | Segurança | Pacientes de terapia e famílias. |
| **Em Alto Mar** | Inspiração | Jovens e nômades digitais. |
| **Bússola** | Precisão | Investidores e profissionais corporativos. |
| **Farol** | Orientação | Imigrantes em situação de crise ou dúvida. |
| **Linha do Horizonte** | Simplicidade | Usuários que buscam consultoria rápida. |

---
> **Nota de Implementação:** Independentemente do conceito escolhido, todos devem integrar a API do **Stripe Connect** para garantir a automação do split de pagamentos (70% profissional / 30% Navegantes).

