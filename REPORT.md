# Lab 8 — Report

Paste your checkpoint evidence below. Add screenshots as image files in the repo and reference them with `![description](path)`.

## Task 1A — Bare agent
<img width="1423" height="564" alt="image" src="https://github.com/user-attachments/assets/005ad53c-cb73-422a-b54c-6b8fc17e4a4b" />

<!-- Paste the agent's response to "What is the agentic loop?" and "What labs are available in our LMS?" -->

## Task 1B — Agent with LMS tools
<img width="1270" height="848" alt="image" src="https://github.com/user-attachments/assets/fdfa93d3-95d0-4998-a671-0d6011437a48" />

<!-- Paste the agent's response to "What labs are available?" and "Describe the architecture of the LMS system" -->

## Task 1C — Skill prompt
<img width="815" height="631" alt="image" src="https://github.com/user-attachments/assets/0602aef5-cc29-4d9e-b098-9f9b879881d6" />

<!-- Paste the agent's response to "Show me the scores" (without specifying a lab) -->

## Task 2A — Deployed agent
<img width="1465" height="816" alt="image" src="https://github.com/user-attachments/assets/406632e5-5d32-4999-97b1-c444ca96d347" />

<!-- Paste a short nanobot startup log excerpt showing the gateway started inside Docker -->

## Task 2B — Web client
<img width="1533" height="836" alt="image" src="https://github.com/user-attachments/assets/bbe07956-3b0e-4564-8fea-fa7354206a56" />

<!-- Screenshot of a conversation with the agent in the Flutter web app -->

## Task 3A — Structured logging

Happy-path log excerpt (request_started → request_completed with status 200):                                                                                                                                       
                                                                                                                                                                                                                        
  {"_msg":"request_started","_time":"2026-03-27T13:39:19.872167168Z","event":"request_started","method":"GET","path":"/items/","trace_id":"bee06c4fabbfa22ba7202e1615b99231","span_id":"94a1b0bf3bc20db9","service.name"
  :"Learning Management Service","severity":"INFO"}                                                                                                                                                                     
                                                                                                                                                                                                                        
  {"_msg":"auth_success","_time":"2026-03-27T13:39:19.882909952Z","event":"auth_success","trace_id":"bee06c4fabbfa22ba7202e1615b99231","span_id":"94a1b0bf3bc20db9","service.name":"Learning Management                 
  Service","severity":"INFO"}                                                                                                                                                                                         
                                                                                                                                                                                                                        
  {"_msg":"db_query","_time":"2026-03-27T13:39:19.890270464Z","event":"db_query","operation":"select","table":"item","trace_id":"bee06c4fabbfa22ba7202e1615b99231","span_id":"94a1b0bf3bc20db9","service.name":"Learning
   Management Service","severity":"INFO"}                                                                                                                                                                             
                                                                                                                                                                                                                        
  {"_msg":"request_completed","_time":"2026-03-27T13:39:19.99407744Z","event":"request_completed","method":"GET","path":"/items/","status":"200","duration_ms":"121","trace_id":"bee06c4fabbfa22ba7202e1615b99231","span
  _id":"94a1b0bf3bc20db9","service.name":"Learning Management Service","severity":"INFO"}                                                                                                                             
                                                                                                                                                                                                                        
  Error-path log excerpt (db_query with error when PostgreSQL stopped):                                                                                                                                                 
                                                                                                                                                                                                                      
  {"_msg":"request_started","_time":"2026-03-27T14:08:17.707687424Z","event":"request_started","method":"GET","path":"/items/","trace_id":"91603eba5f8293bcd9a59ff56bb6e5aa","span_id":"b1c4e8a57bc0c665","service.name"
  :"Learning Management Service","severity":"INFO"}                                                                                                                                                                     
                                                                                                                                                                                                                        
  {"_msg":"auth_success","_time":"2026-03-27T14:08:17.708615936Z","event":"auth_success","trace_id":"91603eba5f8293bcd9a59ff56bb6e5aa","span_id":"b1c4e8a57bc0c665","service.name":"Learning Management                 
  Service","severity":"INFO"}                                                                                                                                                                                         
                                                                                                                                                                                                                        
  {"_msg":"db_query","_time":"2026-03-27T14:08:17.757910272Z","event":"db_query","operation":"select","table":"item","error":"(sqlalchemy.dialects.postgresql.asyncpg.InterfaceError) <class                            
  'asyncpg.exceptions._base.InterfaceError'>: connection is closed\n[SQL: SELECT item.id, item.type, item.parent_id, item.title, item.description, item.attributes, item.created_at FROM                              
  item]","trace_id":"91603eba5f8293bcd9a59ff56bb6e5aa","span_id":"b1c4e8a57bc0c665","service.name":"Learning Management Service","severity":"ERROR"}                                                                    
                                                                                                                                                                                                                      
  VictoriaLogs query screenshot:                                                                                                                                                                                        
   
  <img width="1532" height="275" alt="image" src="https://github.com/user-attachments/assets/31fce299-1785-4550-b704-fa6d8fab7f17" />
                                                                                                                                                                                             
                                                                                                                                                                                                                      
  Query used: _stream:{service.name=~".*"} AND severity:ERROR    
  
## Task 3B — Traces
healthy trace: <img width="1522" height="763" alt="image" src="https://github.com/user-attachments/assets/431bb3b0-502b-40c6-b703-c88487b48073" />
Error trace: <img width="1536" height="859" alt="image" src="https://github.com/user-attachments/assets/adc3a1c6-7c1d-4321-8ebe-f5d62154c908" />


## Task 3C — Observability MCP tools

Agent response to "Any errors in the last hour?" under normal conditions:                                                                                           
                                                                                                                                                                        
  No, there were no ERROR-level logs recorded in the past hour.                                                                                                         
  The most recent error we found was at 14:08 UTC, which is just over an hour ago.                                                                                      
                                                                                                                                                                        
  Agent response to "Any errors in the last hour?" after stopping PostgreSQL:                                                                                           
                                                                                                                                                                        
  There was **one error in the last hour** (between 15:20 and 15:22 UTC):                                                                                               
                                                                                                                                                                        
  | Time (UTC) | Service | Operation | Error Message | Trace ID |                                                                                                       
  |------------|---------|----------|---------------|----------|                                                                                                        
  | 2026‑03‑27 15:20:41.845 | Learning Management Service | `db_query` (select) | `[Errno -2] Name or service not known` | `81a0e6a2d072220073d01255c1bb9599` |         
                                                                                                                                                 

## Task 4A — Multi-step investigation

 Agent response to "What went wrong?":                                                                                                                                                                                 
                                                                                                                                                                                                                        
  I'll investigate what went wrong in the system.                                                                                                                                                                     
                                                                                                                                                                                                                        
  1. First, checking error count in the last hour...                                                                                                                                                                    
                                                                                                                                                                                                                        
  Found 3 errors in the last hour, all from "Learning Management Service".                                                                                                                                              
                                                                                                                                                                                                                      
  2. Getting error details...                                                                                                                                                                                           
                                                                                                                                                                                                                      
  Error Log Sample:                                                                                                                                                                                                     
  - Timestamp: 2026-03-27T17:42:51 UTC                                                                                                                                                                                
  - Service: Learning Management Service                                                                                                                                                                                
  - Event: db_query                                                                                                                                                                                                   
  - Error: [Errno -2] Name or service not known                                                                                                                                                                         
  - Trace ID: cd497db26c3a04e292a3fd863218bb49                                                                                                                                                                          
                                                                                                                                                                                                                        
  3. Fetching trace details for cd497db26c3a04e292a3fd863218bb49...                                                                                                                                                     
                                                                                                                                                                                                                        
  Span Hierarchy:                                                                                                                                                                                                       
  └── GET /items/ (155ms) [ERROR]                                                                                                                                                                                     
      ├── connect (123ms) [ERROR]                                                                                                                                                                                       
      │   └── socket.gaierror: Name or service not known                                                                                                                                                                
      └── HTTP response (500)                                                                                                                                                                                           
                                                                                                                                                                                                                        
  Root Cause Analysis:                                                                                                                                                                                                  
  - Operation: GET /items/ endpoint                                                                                                                                                                                     
  - What failed: Database connection attempt                                                                                                                                                                            
  - Error: socket.gaierror - "Name or service not known"                                                                                                                                                                
  - Location: asyncpg trying to resolve hostname 'postgres'                                                                                                                                                             
  - Trace ID: cd497db26c3a04e292a3fd863218bb49                                                                                                                                                                          
                                                                                                                                                                                                                        
  The backend cannot resolve the PostgreSQL hostname. The database service may                                                                                                                                          
  be down or the DNS/network configuration is incorrect.

## Task 4B — Proactive health check

<img width="3072" height="1740" alt="4B" src="https://github.com/user-attachments/assets/5e11eda0-5313-400e-9134-7475144a2ccc" />


## Task 4C — Bug fix and recovery

 1. Root cause: FastAPI exception handler missing request: Request parameter                                                                                                                                           
                                                                                                                                                                                                                        
  2. Code fix:                                                                                                                                                                                                        
  - async def unhandled_exception_handler(exc: Exception):                                                                                                                                                              
  + async def unhandled_exception_handler(request: Request, exc: Exception):                                                                                                                                          
                                                                                                                                                                                                                        
  3. Post-fix response: Now shows real database error instead of TypeError                                                                                                                                              
                                                                                                                                                                                                                        
  4. Healthy follow-up: Errors in last hour: 0 - Status: ✅ All systems healthy
