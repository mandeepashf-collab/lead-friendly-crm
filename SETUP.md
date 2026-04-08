# Lead Friendly CRM - Complete Setup Guide

## 🚀 Quick Start (5 minutes)

### 1. Prerequisites
- Node.js 18+ installed (`node --version`)
- - Git installed
  - - A Supabase account (free at supabase.com)
   
    - ### 2. Clone and Install
    - ```bash
      git clone https://github.com/mandeepashf-collab/lead-friendly-crm.git
      cd lead-friendly-crm
      npm install
      ```

      ### 3. Set Up Supabase
      1. Go to **supabase.com** → Create new project (free tier)
      2. 2. Wait for DB to initialize (~2 min)
         3. 3. Get your credentials:
            4.    - Project URL: Settings → General → Project URL
                  -    - Anon Key: Settings → API → anon public key
                       - 4. Create `.env.local`:
                         5. ```env
                            NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
                            NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key-here
                            ```

                            ### 4. Run Migrations
                            ```bash
                            npm run db:setup
                            ```

                            ### 5. Start Development Server
                            ```bash
                            npm run dev
                            ```

                            Open **http://localhost:3000** 🎉

                            ---

                            ## 📁 Project Structure

                            ### Core Directories
                            - `/app` - Next.js 14 app directory (pages + API routes)
                            - - `/components` - React components (shadcn/ui + custom)
                              - - `/lib` - Utilities (Supabase client, hooks, types)
                                - - `/supabase` - Database schema & migrations
                                  - - `/public` - Static assets
                                   
                                    - ### Key Directories
                                    - ```
                                      app/
                                      ├── (dashboard)/          # Protected routes
                                      │   ├── layout.tsx
                                      │   ├── page.tsx          # Dashboard
                                      │   ├── leads/            # Lead management
                                      │   ├── calls/            # Call history
                                      │   ├── conversations/    # Inbox
                                      │   ├── campaigns/        # Campaign management
                                      │   ├── ai-agents/        # Agent builder
                                      │   ├── automations/      # Workflow builder
                                      │   ├── payments/         # Billing
                                      │   ├── branding/         # White-label
                                      │   └── settings/         # Admin settings
                                      ├── api/                  # Backend API routes
                                      │   ├── leads/            # Lead CRUD
                                      │   ├── calls/            # Call management
                                      │   ├── webhooks/         # Retell & Twilio
                                      │   └── auth/             # Authentication
                                      └── auth/                 # Auth pages (login/signup)
                                      ```

                                      ---

                                      ## 🗄️ Database Setup

                                      ### PostgreSQL Schema (Auto-run with migrations)

                                      The following tables will be created:
                                      - **organizations** - Multi-tenant support
                                      - - **users** - Team members
                                        - - **leads** - Contact management
                                          - - **calls** - Call history
                                            - - **conversations** - SMS/Email/Chat threads
                                              - - **messages** - Individual messages
                                                - - **ai_agents** - AI voice bot configs
                                                  - - **campaigns** - Outbound campaigns
                                                    - - **phone_numbers** - Twilio number pool
                                                      - - **appointments** - Calendar events
                                                        - - **workflows** - Automation rules
                                                          - - **invoices** - Billing
                                                            - - **audit_logs** - Activity tracking
                                                             
                                                              - ---

                                                              ## 🔐 Authentication Setup

                                                              We use Supabase Auth with email/password:

                                                              1. **Sign Up**: `/auth/signup`
                                                              2. 2. **Log In**: `/auth/login`
                                                                 3. 3. **Password Reset**: `/auth/reset-password`

                                                                 Middleware in `lib/middleware.ts` protects all `/dashboard/*` routes.

                                                                 ---

                                                                 ## 🔌 Integration Setup

                                                                 ### Retell AI (Voice Agents)
                                                                 1. Create account at **retell.ai**
                                                                 2. 2. Generate API key
                                                                    3. 3. Add to Supabase `organizations.retell_api_key`
                                                                       4. 4. Build agents in Retell dashboard
                                                                          5. 5. API endpoint: `/api/webhooks/retell` (for call callbacks)
                                                                            
                                                                             6. ### Twilio (Telephony)
                                                                             7. 1. Create account at **twilio.com**
                                                                                2. 2. Get Account SID, Auth Token, Phone Number
                                                                                   3. 3. Add to Supabase `organizations.twilio_account_sid` etc.
                                                                                      4. 4. Configure webhook: `your-domain.com/api/webhooks/twilio`
                                                                                         5. 5. Test calls: `POST /api/calls/initiate`
                                                                                           
                                                                                            6. ### n8n (Automation Webhooks)
                                                                                            7. 1. Self-hosted or n8n.cloud
                                                                                               2. 2. Create webhook URLs:
                                                                                                  3.    - Agent Sync: `/api/webhooks/n8n/agent-sync`
                                                                                                        -    - Campaign Trigger: `/api/webhooks/n8n/campaign`
                                                                                                             -    - Call Results: `/api/webhooks/n8n/call-result`
                                                                                                              
                                                                                                                  - ---
                                                                                                                  
                                                                                                                  ## 📦 Core API Routes
                                                                                                                  
                                                                                                                  ### Leads API
                                                                                                                  ```
                                                                                                                  GET  /api/leads              # List all leads
                                                                                                                  POST /api/leads              # Create lead
                                                                                                                  GET  /api/leads/[id]         # Get lead details
                                                                                                                  PUT  /api/leads/[id]         # Update lead
                                                                                                                  DELETE /api/leads/[id]       # Delete lead
                                                                                                                  POST /api/leads/import       # Bulk CSV import
                                                                                                                  ```
                                                                                                                  
                                                                                                                  ### Calls API
                                                                                                                  ```
                                                                                                                  GET  /api/calls              # List calls
                                                                                                                  POST /api/calls              # Log call
                                                                                                                  GET  /api/calls/[id]         # Call details
                                                                                                                  POST /api/calls/initiate     # Start outbound call
                                                                                                                  ```
                                                                                                                  
                                                                                                                  ### Conversations API
                                                                                                                  ```
                                                                                                                  GET  /api/conversations      # List conversations
                                                                                                                  POST /api/conversations      # Create
                                                                                                                  GET  /api/conversations/[id] # Get thread
                                                                                                                  POST /api/conversations/[id]/messages  # Send message
                                                                                                                  ```
                                                                                                                  
                                                                                                                  ### AI Agents API
                                                                                                                  ```
                                                                                                                  GET  /api/agents             # List agents
                                                                                                                  POST /api/agents             # Create agent
                                                                                                                  PUT  /api/agents/[id]        # Update agent
                                                                                                                  POST /api/agents/[id]/sync   # Sync to Retell
                                                                                                                  ```
                                                                                                                  
                                                                                                                  ---
                                                                                                                  
                                                                                                                  ## 🔧 Environment Variables
                                                                                                                  
                                                                                                                  Create `.env.local`:
                                                                                                                  
                                                                                                                  ```env
                                                                                                                  # Supabase
                                                                                                                  NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
                                                                                                                  NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key

                                                                                                                  # Auth
                                                                                                                  NEXTAUTH_SECRET=your-secret-key-here
                                                                                                                  NEXTAUTH_URL=http://localhost:3000

                                                                                                                  # Integrations (optional initially)
                                                                                                                  RETELL_API_KEY=your-retell-key
                                                                                                                  TWILIO_ACCOUNT_SID=your-twilio-sid
                                                                                                                  TWILIO_AUTH_TOKEN=your-twilio-token
                                                                                                                  N8N_WEBHOOK_BASE_URL=https://your-n8n.com

                                                                                                                  # App
                                                                                                                  NEXT_PUBLIC_APP_URL=http://localhost:3000
                                                                                                                  ```
                                                                                                                  
                                                                                                                  ---
                                                                                                                  
                                                                                                                  ## 🧪 Testing
                                                                                                                  
                                                                                                                  ### Test Lead Creation
                                                                                                                  ```bash
                                                                                                                  curl -X POST http://localhost:3000/api/leads \
                                                                                                                    -H "Content-Type: application/json" \
                                                                                                                    -d '{"name":"John Doe","phone":"+14155552671","email":"john@example.com"}'
                                                                                                                  ```
                                                                                                                  
                                                                                                                  ### Test Call Initiation
                                                                                                                  ```bash
                                                                                                                  curl -X POST http://localhost:3000/api/calls/initiate \
                                                                                                                    -H "Content-Type: application/json" \
                                                                                                                    -d '{"lead_id":"uuid","agent_id":"uuid","phone_number":"+14155552671"}'
                                                                                                                  ```
                                                                                                                  
                                                                                                                  ---
                                                                                                                  
                                                                                                                  ## 📚 Key Features (In Order of Completion)
                                                                                                                  
                                                                                                                  ### Phase 1: Foundation (Week 1)
                                                                                                                  - ✅ Next.js setup + TypeScript
                                                                                                                  - - ✅ Supabase auth + database
                                                                                                                    - - ✅ Lead management (CRUD)
                                                                                                                      - - ✅ Dashboard with stats
                                                                                                                       
                                                                                                                        - ### Phase 2: Calling (Week 2)
                                                                                                                        - - [ ] Retell AI agent integration
                                                                                                                          - [ ] - [ ] Twilio telephony setup
                                                                                                                          - [ ] - [ ] Call initiation + logging
                                                                                                                          - [ ] Call history with recordings
                                                                                                                         
                                                                                                                          - [ ] ### Phase 3: Conversations (Week 2-3)
                                                                                                                          - [ ] - [ ] SMS via Twilio
                                                                                                                          - [ ] - [ ] 3-column inbox UI
                                                                                                                          - [ ] - [ ] Real-time message sync
                                                                                                                          - [ ] - [ ] Contact details panel
                                                                                                                         
                                                                                                                          - [ ] ### Phase 4: Advanced (Week 3-4)
                                                                                                                          - [ ] - [ ] Campaign management
                                                                                                                          - [ ] - [ ] Number rotation + cooldown logic
                                                                                                                          - [ ] - [ ] Automation builder
                                                                                                                          - [ ] - [ ] Appointment scheduling
                                                                                                                          - [ ] - [ ] Payments system
                                                                                                                          - [ ] - [ ] White-label branding
                                                                                                                         
                                                                                                                          - [ ] ### Phase 5: Polish (Week 4-5)
                                                                                                                          - [ ] - [ ] Analytics dashboard
                                                                                                                          - [ ] Bulk operations
                                                                                                                          - [ ] - [ ] Mobile app (React Native)
                                                                                                                          - [ ] - [ ] Performance optimization
                                                                                                                          - [ ] - [ ] Compliance features
                                                                                                                         
                                                                                                                          - [ ] ---
                                                                                                                         
                                                                                                                          - [ ] ## 🚢 Deployment
                                                                                                                         
                                                                                                                          - [ ] ### Deploy to Vercel (Recommended)
                                                                                                                          - [ ] ```bash
                                                                                                                          - [ ] npm install -g vercel
                                                                                                                          - [ ] vercel login
                                                                                                                          - [ ] vercel
                                                                                                                          - [ ] ```
                                                                                                                         
                                                                                                                          - [ ] Add environment variables in Vercel dashboard, then:
                                                                                                                          - [ ] ```bash
                                                                                                                          - [ ] vercel --prod
                                                                                                                          - [ ] ```
                                                                                                                         
                                                                                                                          - [ ] ### Deploy to Supabase (Database)
                                                                                                                          - [ ] Already done! Your PostgreSQL DB auto-syncs.
                                                                                                                          - [ ] 
                                                                                                                          ---
                                                                                                                          
                                                                                                                          ## 📖 Useful Links
                                                                                                                          
                                                                                                                          - **Next.js Docs**: https://nextjs.org/docs
                                                                                                                          - - **Supabase Docs**: https://supabase.com/docs
                                                                                                                            - - **shadcn/ui**: https://ui.shadcn.com
                                                                                                                              - - **Retell AI**: https://retell.ai/docs
                                                                                                                                - - **Twilio**: https://www.twilio.com/docs
                                                                                                                                 
                                                                                                                                  - ---
                                                                                                                                  
                                                                                                                                  ## 🆘 Troubleshooting
                                                                                                                                  
                                                                                                                                  ### "Cannot find module" errors
                                                                                                                                  ```bash
                                                                                                                                  rm -rf node_modules package-lock.json
                                                                                                                                  npm install
                                                                                                                                  ```
                                                                                                                                  
                                                                                                                                  ### Supabase connection issues
                                                                                                                                  - Check `.env.local` has correct URL and keys
                                                                                                                                  - - Verify Supabase project is running
                                                                                                                                    - - Check browser console for CORS errors
                                                                                                                                     
                                                                                                                                      - ### Database migration fails
                                                                                                                                      - ```bash
                                                                                                                                        npm run db:push -- --force  # ⚠️ WARNING: This resets DB
                                                                                                                                        ```
                                                                                                                                        
                                                                                                                                        ---
                                                                                                                                        
                                                                                                                                        ## 📞 Support & Next Steps
                                                                                                                                        
                                                                                                                                        1. Follow setup steps above
                                                                                                                                        2. 2. Test lead creation via API
                                                                                                                                           3. 3. Post screenshots in Discord/Slack
                                                                                                                                              4. 4. We'll build features together in real-time
                                                                                                                                                
                                                                                                                                                 5. **You're all set! Let's build something amazing** 🚀
