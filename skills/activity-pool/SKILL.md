---
name: activity-pool
description: Match dictated activities to the curated pool of 1,088 activities across 19 categories with fuzzy matching and disambiguation
---

# Activity Pool — Fuzzy Activity Matcher

You match the user's natural language activity descriptions to the curated activity pool of 1,088 activities across 19 categories. When an exact match isn't found, use fuzzy matching. When ambiguous, ask for clarification.

## Categories and Representative Activities

### Deep Work (Coding, Testing, Architecture, Debugging)
- Write Python backend module
- Build React component
- Implement REST API endpoint
- CC mega-prompt creation / CC execution session
- Write unit/integration tests
- Design microservices architecture
- Debug production memory leak
- PowerShell script development
- Database migration scripts
- Code review (PR review)

### BD / Sales
- Cold outreach email campaign
- Discovery call with prospect
- Write proposal / SOW
- Product demo for client
- Follow-up call after demo
- Negotiate contract terms
- Update CRM pipeline
- Prepare pitch deck
- Attend networking event
- Partnership exploration call

### Content Creation
- Write Substack article draft
- Edit and publish blog post
- Record video walkthrough
- Create social media content
- Design infographic
- Write newsletter
- LinkedIn post drafting
- Podcast guest preparation
- Create case study
- Content calendar planning

### Strategy / Planning
- Weekly planning session
- Quarterly OKR review
- Product roadmap update
- Competitive analysis
- Brainstorm session
- Prioritization matrix exercise
- Budget planning
- Market sizing analysis
- Go-to-market strategy
- Revenue target review

### Meetings / Calls
- Team standup
- 1:1 with direct report
- Client sync call
- Board/advisor meeting
- Vendor evaluation call
- Cross-team coordination
- Interview candidate
- All-hands meeting
- Sprint retrospective
- Investor update call

### Admin / DevOps
- Deploy to production
- Configure CI/CD pipeline
- Set up monitoring/alerting
- DNS/domain configuration
- Server maintenance
- Database backup verification
- SSL certificate renewal
- Cloud infrastructure review
- Docker/container management
- Access permissions audit

### Admin / Email
- Process inbox to zero
- Reply to client emails
- Internal Slack messages
- Schedule meetings via email
- File/organize documents
- Update project boards
- Expense report submission
- Administrative paperwork
- Calendar management
- Vendor correspondence

### Research / Learning
- Read technical documentation
- Take online course module
- Watch conference talk
- Read industry report
- Explore new framework/tool
- Study for certification
- Read book (professional)
- Attend webinar
- Research competitor product
- Review academic paper

### Fitness / Exercise
- Gym — weight training
- Gym — cardio (treadmill/bike)
- Run outdoors
- Yoga session
- Walk (neighborhood/park)
- Stretching routine
- HIIT workout
- Swimming
- Sports (basketball, tennis, etc.)
- Home bodyweight workout

### Family / Kids
- Play with kids (indoor)
- Play with kids (outdoor/park)
- School pickup/dropoff
- Help with homework
- Bedtime routine (stories, tuck-in)
- Family meal together
- Family outing/activity
- Kids bath time
- Drive kids to activity
- Quality time with spouse/partner

### Household / Home
- Cook meal
- Clean kitchen / do dishes
- Laundry (wash/fold/put away)
- Vacuum / mop floors
- Grocery shopping
- Yard work / lawn care
- Home repair / maintenance
- Organize closet/room
- Take out trash/recycling
- Pet care (feed/walk)

### Errands / Outside
- Grocery run
- Bank visit
- Post office / mail package
- Pharmacy pickup
- Car wash / gas station
- Return items to store
- Haircut / grooming appointment
- Pick up dry cleaning
- Visit hardware store
- Drop off / pick up repair

### Personal Development
- Meditation session
- Journal writing
- Therapy session
- Listen to podcast (growth)
- Gratitude practice
- Goal review / reflection
- Read self-improvement book
- Coaching session
- Habit tracking review
- Prayer / spiritual practice

### Personal / Morning
- Morning routine (wash up, dress)
- Make/drink coffee
- Breakfast
- Read morning news
- Morning stretch
- Check phone/notifications
- Plan the day
- Shower
- Skincare routine
- Get kids ready

### Travel / Commute
- Drive to office
- Drive home from office
- Uber/Lyft ride
- Airport transit
- Flight
- Train/bus commute
- Walk to destination
- Road trip segment
- Wait at airport/station
- Drive kids to school

### Social / Entertainment
- Watch TV/Netflix
- Browse YouTube
- Social media scrolling
- Dinner with friends
- Game night
- Phone call with friend/family
- Movie (theater or home)
- Video games
- Attend social event
- Read for leisure

### Finance / Legal
- Review bank statements
- Pay bills
- Tax preparation
- Invoice clients
- Review insurance policies
- Meet with accountant
- Legal document review
- Budget tracking update
- Investment review
- Contract signing

### Health / Medical
- Doctor appointment
- Dentist appointment
- Physical therapy session
- Pick up prescription
- Mental health check-in
- Lab work / blood draw
- Eye exam
- Urgent care visit
- Health insurance paperwork
- Specialist consultation

### Sleep
- Night sleep
- Power nap (20-30 min)
- Rest/recovery after illness

## Fuzzy Matching Rules

1. **Exact match**: If the user's description matches an activity name exactly (case-insensitive), use it.
2. **Keyword match**: Match on key nouns/verbs. "worked on Python code" → "Write Python backend module"
3. **Category match**: If the activity clearly belongs to a category but no specific match, use the closest representative. "did some tidying up" → "Clean kitchen / do dishes" or "Organize closet/room"
4. **Partial match**: "CC session" → "CC execution session" or "CC mega-prompt creation" — ask which.

## Disambiguation

When an activity is ambiguous, ask for clarification:

- **"gym"** → "Gym — weight training or cardio? Or a different type of workout?"
- **"kids"** → "Playing with kids? Indoor or outdoor? Or a different kids activity (homework, bedtime, dropoff)?"
- **"meeting"** → "What kind of meeting? Client sync, team standup, 1:1, or something else?"
- **"writing"** → "Content writing (blog/Substack) or work writing (docs/proposals)?"
- **"call"** → "Client call, team call, personal call, or sales call?"

## No Match Found

If no match exists in the pool:
1. Accept the raw input as-is
2. Assign the most likely category based on keywords
3. Suggest: "I don't have '[activity]' in the pool. I'll log it as-is under [Category]. Want to add it to your activity pool for future matching?"

## Usage

This skill is called internally by the daily-log skill during parsing. It is not invoked directly by the user. When daily-log processes a dictation entry, it calls activity-pool to:
1. Find the best matching activity name
2. Confirm or assign the category
3. Return the standardized activity name and category
