# SakhiSutra

**A voice-first, multi-agent AI co-manager that helps women's self-help groups (SHGs) price, split, track and pay bulk orders fairly.**

Tech4Bharat 2026 (GAVS) · Theme: Youth and Women · Problem statement 8.1
Team: Nithin Eashwar and Omesh Sengar, RV College of Engineering, Bengaluru

> Status: idea stage. This repository holds our research and system design. Prototype development begins after shortlisting (23 Nov – 15 Dec 2026).

---

## The problem

When an SHG takes on a bulk order for garments, food or crafts, one leader coordinates 10–15 members working from home. Today:

- work is divided verbally and tracked in a notebook
- quality is checked only when all output comes together, when it is too late to fix
- payment is split by estimate
- members underprice by 30–40% because material, labour and margin are never calculated

The result is missed deadlines, rejected batches and members who stop trusting the accounts. Many members have low literacy, so they cannot check their own records.

## The solution

SakhiSutra runs inside WhatsApp or Telegram and works through voice notes in local languages. There is no new app to install, and no reading or typing is needed. Five agents, coordinated by an orchestrator, follow an order through its full lifecycle:

| Agent | What it does |
|---|---|
| **Price** | Breaks down material, labour at fair local wages, overheads and margin, then suggests a fair quote with its reasoning |
| **Split** | Divides the order by each member's capacity and skills, and sends each task as a voice note |
| **Track** | Takes voice check-ins, predicts delays and suggests reallocation |
| **Check** | Compares photo samples with a reference piece the group agreed on, and flags differences for the group to review |
| **Pay** | Calculates each member's share from accepted output and proposes the payout for group approval |

Any member can ask by voice "How much have I earned?" and hear her own record, in her own language.

### Design principles

- The group approves every payout. Agents suggest; people decide.
- Each member sees and hears only her own record. The leader sees the group view.
- Every price, share and warning comes with a plain-language explanation.
- Quality is a flag for the group, never an automatic rejection.
- The ledger is append-only and hash-chained, so any change is visible.

## Architecture

```
Member's phone (WhatsApp / Telegram: voice notes, photos)
        │
        ▼
Bhashini (speech-to-text, text-to-speech, translation)
        │
        ▼
Orchestrator agent (Lyzr) ── LLM (Gemini / Groq free tier)
        │
        ├── Price agent
        ├── Split agent
        ├── Track agent
        ├── Check agent (vision-language model)
        └── Pay agent
        │
        ▼
Supabase (orders, members, hash-chained ledger) ──► Leader / federation dashboard
```

Every component runs on a free tier for the prototype.

## Where SakhiSutra fits in the existing landscape

| System | What it does | What it does not do |
|---|---|---|
| [LokOS](https://www.civilsdaily.com/news/lokos-digital-backbone-for-rural-livelihoods/) (DAY-NRLM) | Digitises SHG members, savings, loans and repayments for 10 crore+ members | Bulk-order pricing, work allocation or production payouts |
| [eSARAS](https://www.pib.gov.in/FactsheetDetails.aspx?id=150656) (DAY-NRLM) | Online marketplace for SHG products, on ONDC; premium collection for institutional and corporate buyers (2026) | Helping a group fulfil a large order reliably |
| [NABARD EShakti](https://www.teriin.org/news/nabard-commences-digitisation-shgs) | Digitised SHG records to make groups bankable | Order fulfilment |

**The gap:** the government already digitises SHG finances and sells their products. Nothing manages the step in between: splitting a bulk order fairly across members and paying them transparently. SakhiSutra is that fulfilment layer, and it complements these systems.

## Research references

- **Scale:** Ministry of Rural Development (Oct 2025): 90.9 lakh SHGs and 10 crore+ rural households mobilised under DAY-NRLM. [PIB document](https://static.pib.gov.in/WriteReadData/specificdocs/documents/2025/oct/doc20251023672401.pdf)
- **Constraints of SHG enterprises:** Singh, Jirli and Rai, *Study of Perceived Constraints Faced By Women Self Help Group Entrepreneurs*, Indian Journal of Extension Education (2014). Price fluctuation and dependence on intermediaries ranked among the most serious constraints. This motivates the Price agent. [Link](https://epubs.icar.org.in/index.php/IJEE/article/view/144293)
- **Voice for low-literacy users:** Mudliar, Donner and Thies, *Emergent Practices Around CGNet Swara, A Voice Forum for Citizen Journalism in Rural India*, ICTD 2012 (Microsoft Research India). Phone-based voice forums worked for low-literacy rural users. This supports our voice-first design. [Paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ictd12-swara.pdf)

## Technology

- [Bhashini / ULCA](https://github.com/bhashini-dibd/ulca): Government of India language AI for Indian-language speech and translation
- [Lyzr AI](https://www.lyzr.ai): agent orchestration (hackathon credits)
- [Telegram Bot API](https://core.telegram.org/bots/api): member-facing interface
- [Supabase](https://supabase.com/docs): database and ledger

## Business model

1. **Free for every SHG**, which drives adoption.
2. **Marketplace commission:** groups with a strong on-time record become verified suppliers for bulk buyers (corporate gifting, retailers, D2C brands), and we earn a small fee per order.
3. **Federation dashboards:** paid monitoring for SHG federations and livelihood missions.
4. **Credit history (later):** verified delivery and payment records that banks linked to SHGs can lend against.

## Roadmap

| When | Milestone |
|---|---|
| 15 Nov 2026 | Idea submitted |
| 23 Nov – 15 Dec 2026 | Working prototype: Telegram bot with Kannada and Hindi voice, all five agents, ledger and leader dashboard |
| During the build | Planned field sessions with a local SHG to test pricing, splitting and payouts on real orders |
| 27 Dec 2026 | Live demo at the finale: one full order, from quote to group-approved payout |
