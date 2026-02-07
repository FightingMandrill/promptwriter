# Gemini Prompting Guide Summary

## Prompt Writing Rules

1. **Use Natural Language**  
   Write prompts like you’re talking to a human. Use full sentences.

2. **Be Specific & Iterative**  
   Always include a task verb (e.g., summarize, draft, rephrase). Refine based on output.

3. **Avoid Complexity**  
   Be brief but specific. Cut jargon.

4. **Use Persona + Task + Context + Format**  
   Combine at least two of these to improve results.

5. **Ground in Your Content**  
   Reference documents from Google Drive using `@filename`.

6. **Refine with Gemini**  
   Prefix your draft with “Make this a power prompt:” to get improvement suggestions.

---

## Role-Based Prompt Examples

### Administrative Support

#### Example: Plan Agendas for Team Offsite
**Goal**: Create a structured agenda for a multi-day team offsite, incorporating bonding and strategy time.

**Starting Prompt**:  
`I am an executive administrator to a team director. Our newly formed team now consists of content marketers, digital marketers, and product marketers. We are gathering for the first time at a three-day offsite in Washington, DC. Plan activities for each day that include team bonding activities and time for deeper strategic work. Create a sample agenda for me.`

**Use-Case + Prompt**:  
Plan an agenda for a 3-day offsite that mixes bonding and work sessions:  
`Plan a 3-day agenda in Washington DC for a new marketing team offsite. Include team bonding and strategic work.`

**Follow-up Prompt (Refinement)**:  
`Suggest three different icebreaker activities that encourage people to learn about their teammates’ preferred working styles, strengths, and goals. Make sure the icebreaker ideas are engaging and can be completed by a group of 25 people in 30 minutes or less.`

**Format Prompt**:  
`Organize this agenda in a table format. Include one of your suggested icebreakers for each day.`



### Communications

#### Example: Draft Executive Update
**Goal**: Summarize and format a quarterly update for executive stakeholders.

**Starting Prompt**:  
`You are a communications lead supporting a VP. Draft a 250-word update summarizing key milestones from Q1.`

**Use-Case + Prompt**:  
Create concise executive summaries from internal updates:  
`Summarize the attached team OKR doc into a 250-word stakeholder update for execs. Use an optimistic and formal tone.`

---

### Customer Service

#### Example: Standardize Responses
**Goal**: Turn a draft into a reusable template for customer support.

**Starting Prompt**:  
`Refine this support reply to be more empathetic and clear. Then turn it into a reusable template.`

**Use-Case + Prompt**:  
Standardize high-quality replies for recurring queries:  
`Convert this support email into a reusable template. Maintain tone but improve clarity.`

---

### Executives

#### Example: Define Strategic Vision
**Goal**: Articulate a clear long-term vision for your org based on notes.

**Starting Prompt**:  
`Based on these meeting notes, write a 2-year vision statement for the org. Keep it aspirational but grounded.`

**Use-Case + Prompt**:  
Turn raw thinking into structured leadership comms:  
`Draft a vision statement for our engineering org for FY26 based on this whiteboard summary.`

---

### Frontline Management

#### Example: Weekly Team Update
**Goal**: Write a weekly update for your team from status notes.

**Starting Prompt**:  
`Draft a weekly update from these meeting notes. Use a friendly and appreciative tone.`

**Use-Case + Prompt**:  
Quickly synthesize updates into a message for the team:  
`Create a weekly recap email to my team from these bullet points.`



### Human Resources

#### Example: Draft Internal Policy Summary  
**Goal**: Turn long-form HR policies into clear internal summaries.

**Starting Prompt**:  
`Summarize this new benefits policy into a concise doc for employees. Include key dates and contact info.`

**Use-Case + Prompt**:  
Make HR documents employee-friendly:  
`Summarize our 2024 health benefits update into a one-pager for internal communication.`

---

### Marketing

#### Example: Generate Campaign Ideas  
**Goal**: Brainstorm creative campaigns for a product launch.

**Starting Prompt**:  
`Generate 5 creative campaign ideas to launch our new gaming headphones. Target casual gamers.`

**Use-Case + Prompt**:  
Stimulate creative ideation at scale:  
`Brainstorm 5 marketing concepts for launching a new line of eco-friendly notebooks.`

---

### Project Management

#### Example: Track Project Risks  
**Goal**: Identify and summarize project risks from notes.

**Starting Prompt**:  
`Identify potential risks from this project kickoff transcript. Summarize them in a bullet list.`

**Use-Case + Prompt**:  
Extract risks, blockers, or dependencies from meetings:  
`List project risks based on these notes. Use categories: technical, team, external.`

---

### Sales

#### Example: Write Follow-Up Email  
**Goal**: Craft a follow-up email after a client meeting.

**Starting Prompt**:  
`Draft a follow-up email summarizing today’s meeting with Acme Corp. Mention key priorities discussed.`

**Use-Case + Prompt**:  
Standardize timely follow-ups that move deals forward:  
`Write a polite follow-up email to a potential client. Mention our last meeting’s key points.`

---

### Small Business Owners and Entrepreneurs

#### Example: Market Research Summary  
**Goal**: Summarize competitor reviews for insights.

**Starting Prompt**:  
`Analyze and summarize common themes in these 50 customer reviews from competitors.`

**Use-Case + Prompt**:  
Extract trends from competitor data:  
`Summarize what users like and dislike from 50 Amazon reviews of competing products.`

---

### Startup Leaders

#### Example: Fundraising Narrative  
**Goal**: Create a fundraising pitch narrative from raw notes.

**Starting Prompt**:  
`Turn these bullet points into a compelling fundraising narrative for Series A.`

**Use-Case + Prompt**:  
Turn vision into investor-ready material:  
`Draft a Series A story pitch based on these business model and traction notes.`

