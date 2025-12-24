<!--
SYNC IMPACT REPORT
==================
Version Change: UNVERSIONED (template) → 1.0.0 (initial constitution)

Modified Principles:
- Replaced generic [PRINCIPLE_*] placeholders with Phase IV specific principles
- Added: Agentic First, Spec Driven, Tool Native, Documentation First

Added Sections:
- Architectural Constraints (Containerization, Kubernetes Deployment, AI DevOps Integration)
- Security Constraints (Secrets Management, Image Security, Network Security)
- Quality Constraints (Testing Requirements, Validation Gates, Performance Requirements)
- Documentation Constraints (Mandatory Artifacts, Documentation Standards)
- Workflow Constraints (Phase Gates, Iteration Rules, Approval Gates)
- Exception Handling (Tool Unavailability, Deployment Failures, Requirement Conflicts)
- Success Metrics (Technical Metrics, Process Metrics, Learning Metrics)
- Review and Governance (Review Schedule, Constitution Amendments, Version Control)
- Enforcement (Violation Handling, Audit Trail)
- Continuous Improvement (Feedback Loops, Knowledge Transfer)

Removed Sections: None (all generic placeholders replaced)

Templates Requiring Updates:
✅ .specify/templates/plan-template.md - Constitution Check section compatible
✅ .specify/templates/spec-template.md - Requirements alignment compatible
✅ .specify/templates/tasks-template.md - Task categorization compatible
⚠ .claude/commands/*.md - Review for any hardcoded references to generic principles

Follow-up TODOs: None - all placeholders resolved

Rationale for Version 1.0.0:
- Initial constitution creation from comprehensive user specification
- Establishes Phase IV: Local Kubernetes Deployment governance framework
- All principles, constraints, and governance rules fully defined
- No prior version existed (template state)
-->

# Todo Chatbot - Phase IV: Local Kubernetes Deployment Constitution

## Core Principles

### I. Agentic First

**Mandate**: AI agents are primary actors, humans are reviewers

AI tools MUST be the primary mechanism for code generation and DevOps operations. Manual coding is prohibited unless all AI tool options have been exhausted and documented.

**Rules**:
- No manual coding permitted - all code generation via AI tools
- Any manual code addition requires justification and re-generation
- AI tool priority order: Gordon (Docker) → kubectl-ai (Kubernetes) → kagent (K8s operations) → Claude Code (Implementation)
- Traditional CLI commands are last resort only

**Rationale**: Maximizes learning from AI DevOps tools, ensures consistency, creates reproducible prompts and patterns for future phases.

### II. Spec Driven

**Mandate**: Specification precedes implementation

All implementation work MUST follow the strict workflow: `/specify` → `/plan` → `/tasks` → `/implement`. No implementation may begin without completed specification artifacts.

**Rules**:
- Implementation must strictly follow the SDD workflow sequence
- No implementation without completed specification
- All changes must reference specification documents
- Deviations require specification amendments before proceeding

**Rationale**: Prevents scope creep, ensures alignment with requirements, creates clear acceptance criteria, maintains audit trail of decisions.

### III. Tool Native

**Mandate**: Use AI DevOps tools as primary interface

Gordon, kubectl-ai, and kagent MUST be attempted first for all Docker and Kubernetes operations. Traditional CLI commands are fallback only when AI tools are unavailable or fail after documented attempts.

**Rules**:
- Priority order strictly enforced: Gordon → kubectl-ai → kagent → Claude Code → Manual CLI
- Fallback to traditional CLI only when AI tools unavailable
- All AI tool attempts must be documented with prompts and outputs
- Manual interventions require justification in documentation

**Rationale**: Demonstrates AI-first DevOps approach, builds institutional knowledge of prompt engineering, validates tool effectiveness.

### IV. Documentation First

**Mandate**: Document decisions, prompts, and iterations in real-time

Every AI interaction, architectural decision, and implementation iteration MUST be logged as it occurs. Review phases require complete audit trails.

**Rules**:
- Every AI interaction must be logged (prompt, tool, response, iterations, decision)
- Prompt History Records (PHR) mandatory for all development work
- Architecture Decision Records (ADR) required for significant decisions
- Iteration logs must capture refinement reasoning

**Rationale**: Enables reproducibility, supports learning and improvement, provides compliance evidence, facilitates knowledge transfer.

## Architectural Constraints

### Containerization

**Immutable Rules**:

1. **Multi-stage builds are mandatory**
   - Rationale: Minimize image size and attack surface
   - Exception: None

2. **Alpine-based images required**
   - Rationale: Lightweight production deployments
   - Exception: Only if specific dependency requires full OS (must be documented)

3. **Health checks must be implemented**
   - Rationale: Kubernetes liveness/readiness probes required
   - Exception: None

4. **No hardcoded secrets or credentials**
   - Rationale: Security best practice
   - Enforcement: ConfigMaps/Secrets only
   - Exception: None

5. **Layer caching must be optimized**
   - Rationale: Fast rebuild times
   - Enforcement: Copy dependency files before source code

**Forbidden Practices**:
- Root user in production containers
- Latest tag in production
- Secrets in environment variables
- Unnecessary packages in final image
- Missing .dockerignore

### Kubernetes Deployment

**Immutable Rules**:

1. **Helm charts mandatory for all deployments**
   - Rationale: Templating, versioning, and reproducibility
   - Exception: None

2. **Resource limits must be defined**
   - Rationale: Prevent resource starvation
   - Enforcement: CPU and memory limits required
   - Exception: None

3. **Minimum 2 replicas for production**
   - Rationale: High availability
   - Enforcement: Frontend: 2, Backend: 2+
   - Exception: Development/testing only

4. **Namespaces isolate workloads**
   - Rationale: Multi-tenancy and organization
   - Enforcement: `todo-app` namespace required

5. **Services use ClusterIP by default**
   - Rationale: Security - minimize external exposure
   - Exception: Frontend can use NodePort for Minikube

6. **ConfigMaps for configuration**
   - Rationale: Separate config from code
   - Enforcement: No hardcoded URLs or settings

**Best Practices Enforced**:
- Liveness and readiness probes mandatory
- Rolling update strategy
- Pod disruption budgets for critical services
- Horizontal Pod Autoscaler (HPA) for scalable services
- Labels and annotations for observability

**Forbidden Practices**:
- Privileged containers
- Host network mode
- Mutable tags (e.g., :latest)
- Direct pod access from outside cluster
- Secrets in plain ConfigMaps

### AI DevOps Integration

**Immutable Rules**:

1. **Gordon must be attempted first for Docker operations**
   - Command pattern: `docker ai '<natural language instruction>'`
   - Fallback: Claude Code generation → Manual CLI (last resort)

2. **kubectl-ai for all Kubernetes manifest generation**
   - Command pattern: `kubectl-ai '<natural language instruction>'`
   - Fallback: Manual YAML creation documented

3. **kagent for cluster optimization and analysis**
   - Command pattern: `kagent '<analysis request>'`
   - Use cases: health checks, resource optimization, troubleshooting

4. **All AI interactions must be documented**
   - Documentation required: Original prompt, AI response/output, Iterations performed, Final result applied, Human review decision

**Prompt Engineering Standards**:

Clarity:
- Be specific about desired outcome
- Include technical constraints
- Specify format requirements

Context:
- Reference technology stack
- Mention environment (production/development)
- State performance requirements

Iteration:
- Refine prompts based on output
- Document why refinement was needed
- Track improvement across iterations

## Security Constraints

### Secrets Management

- **No secrets in version control**
  - Enforcement: .gitignore must include .env, secrets/

- **Use Kubernetes Secrets for sensitive data**
  - Enforcement: Base64 encoding minimum
  - Recommendation: External secret management (future phase)

- **Rotate credentials regularly**
  - Enforcement: Document rotation procedure

### Image Security

- **Scan images for vulnerabilities**
  - Tool: docker scan or trivy
  - Enforcement: Critical vulnerabilities must be addressed

- **Use official base images only**
  - Allowed sources: Docker Hub official, Alpine

- **Run as non-root user**
  - Enforcement: USER directive in Dockerfile

### Network Security

- **Minimize exposed ports**
  - Enforcement: Only expose required service ports

- **Use network policies (future enhancement)**
  - Note: Minikube may require CNI plugin

## Quality Constraints

### Testing Requirements

**Pre-Deployment**:
- Local container testing mandatory
- Health endpoint verification required
- Integration testing (frontend ↔ backend)

**Post-Deployment**:
- Smoke tests after Helm install
- End-to-end functionality verification
- Resource consumption monitoring

### Validation Gates

**Dockerfile**:
- Lint with hadolint
- Build completes without errors
- Image size within acceptable range

**Helm Charts**:
- `helm lint` passes
- `helm template` renders without errors
- Values schema validated

**Kubernetes**:
- All pods reach Running state
- Services have endpoints
- No CrashLoopBackOff

### Performance Requirements

**Build Times**:
- Docker build < 5 minutes
- Helm install < 2 minutes
- Pod startup < 30 seconds

**Resource Limits**:

Backend:
- Requests: CPU 250m, Memory 256Mi
- Limits: CPU 500m, Memory 512Mi

Frontend:
- Requests: CPU 100m, Memory 128Mi
- Limits: CPU 200m, Memory 256Mi

## Documentation Constraints

### Mandatory Artifacts

**Specification**:
- speckit.specify
- speckit.plan
- speckit.tasks
- speckit.constitution (this file)

**Technical**:
- README.md - Project overview
- DEPLOYMENT.md - Step-by-step guide
- ARCHITECTURE.md - System design
- AI_DEVOPS_GUIDE.md - Tool usage patterns

**Operational**:
- TROUBLESHOOTING.md - Common issues
- RUNBOOK.md - Operational procedures
- ITERATIONS.md - Development log

### Documentation Standards

**Format**: Markdown

**Structure**:
- Clear headings hierarchy
- Code blocks with syntax highlighting
- Command examples with expected output
- Troubleshooting decision trees

**Content**:
- Prerequisites clearly stated
- Assumptions documented
- Commands copy-pasteable
- Verification steps included

**AI Interactions Template**:
```
**Prompt:** <original prompt>
**Tool:** <Gordon/kubectl-ai/kagent/Claude Code>
**Response:** <output received>
**Iterations:** <count>
**Final Decision:** <applied/rejected/modified>
**Rationale:** <reasoning>
```

## Workflow Constraints

### Phase Gates

**Phase 1: Setup**
- Entry Criteria: None
- Exit Criteria:
  - All tools installed and verified
  - Minikube cluster running
  - Gordon, kubectl-ai, kagent functional
- Deliverables: Environment checklist

**Phase 2: Containerization**
- Entry Criteria: Phase 1 complete
- Exit Criteria:
  - Both Docker images built
  - Local testing successful
  - Health checks verified
- Deliverables: Dockerfiles, Images

**Phase 3: Helm Charts**
- Entry Criteria: Phase 2 complete
- Exit Criteria:
  - Charts pass linting
  - Templates render correctly
  - Values validated
- Deliverables: Helm charts

**Phase 4: Deployment**
- Entry Criteria: Phase 3 complete
- Exit Criteria:
  - All pods Running
  - Services accessible
  - End-to-end tests pass
- Deliverables: Deployed applications

**Phase 5: Optimization**
- Entry Criteria: Phase 4 complete
- Exit Criteria:
  - Cluster health analyzed
  - Resources optimized
  - Autoscaling verified
- Deliverables: Optimization report

**Phase 6: Documentation**
- Entry Criteria: Phase 5 complete
- Exit Criteria:
  - All mandatory docs created
  - AI interactions logged
  - Runbooks completed
- Deliverables: Complete documentation

### Iteration Rules

**Max iterations per task**: 5

**Iteration Triggers**:
- AI output doesn't meet requirements
- Validation/testing fails
- Security issues discovered
- Performance below threshold

**Iteration Documentation Required**:
- Iteration number
- Reason for iteration
- Modified prompt/approach
- Outcome
- Lessons learned

### Approval Gates

**Technical Review**:
- Reviewer: Human
- Criteria: Meets specification, Follows best practices, Security compliant, Performance acceptable

**Documentation Review**:
- Reviewer: Human
- Criteria: Complete and accurate, Reproducible procedures, All AI interactions logged

## Exception Handling

### Tool Unavailability

**Gordon Unavailable**:
- Fallback Order:
  1. Claude Code Dockerfile generation
  2. Manual Dockerfile creation (documented)
- Documentation Required: Why Gordon was unavailable

**kubectl-ai Unavailable**:
- Fallback Order:
  1. Claude Code manifest generation
  2. Manual YAML creation (documented)
- Documentation Required: Alternative approach reasoning

**kagent Unavailable**:
- Fallback Order:
  1. kubectl top and describe commands
  2. Manual cluster analysis (documented)
- Documentation Required: Analysis methodology

### Deployment Failures

**Pod CrashLoopBackOff**:
- Diagnostic Steps:
  1. kubectl-ai 'check why pods are failing'
  2. kubectl describe pod
  3. kubectl logs
  4. kagent 'analyze pod failure patterns'
- Resolution Required: Root cause identified and fixed

**Service Unreachable**:
- Diagnostic Steps:
  1. Check service endpoints
  2. Verify pod labels match selectors
  3. Test connectivity from within cluster
- Resolution Required: Service accessible

**Resource Constraints**:
- Diagnostic Steps:
  1. kagent 'analyze cluster health'
  2. kubectl top nodes/pods
  3. Review resource requests/limits
- Resolution Required: Resources balanced

### Requirement Conflicts

**Resolution Process**:
1. Document the conflict
2. Consult speckit.specify for priority
3. Propose resolution options
4. Get human approval
5. Update constitution if needed

## Success Metrics

### Technical Metrics

**Deployment**:
- Pod crash rate: Target 0%, Threshold < 5%
- Deployment success rate: Target 100%, Threshold > 95%
- Service availability: Target 100%, Threshold > 99%

**Performance**:
- API response time: Target < 200ms (p95), Threshold < 500ms (p95)
- Frontend load time: Target < 2s, Threshold < 3s
- Resource utilization: Target 50-70%, Threshold < 90%

### Process Metrics

**AI Adoption**:
- AI-generated code %: Target 100%, Threshold > 95%
- Manual interventions: Target 0, Threshold < 3
- Tool usage coverage: Target All tools used, Threshold At least Gordon + kubectl-ai

**Documentation**:
- AI interactions documented: Target 100%, Threshold 100%
- Mandatory docs completed: Target 100%, Threshold 100%
- Runbook accuracy: Target Reproducible, Threshold Reproducible with minor adjustments

### Learning Metrics

**Iterations**:
- Average iterations per task: Target < 3, Benchmark for future phases
- Prompt refinement quality: Target Improving over time, Measurement Subjective assessment
- AI tool effectiveness: Target High confidence in outputs, Measurement Acceptance rate of AI suggestions

## Review and Governance

### Review Schedule

**Phase Completion**:
- Trigger: Each phase gate completed
- Reviewers: Technical Lead, DevOps Engineer
- Focus: Compliance with constitution, Quality gates met

**Post-Deployment**:
- Trigger: Successful deployment to Minikube
- Reviewers: Full team
- Focus: End-to-end functionality, Documentation completeness

### Constitution Amendments

**Trigger Conditions**:
- Fundamental constraint is blocking progress
- New best practice discovered
- Tool limitations require workaround
- Security vulnerability in current approach

**Amendment Process**:
1. Document proposed change
2. Justify with evidence
3. Impact analysis
4. Team approval
5. Update constitution with version bump

**Version Control**:
- Format: MAJOR.MINOR.PATCH
- MAJOR: Fundamental principle change
- MINOR: New constraint added
- PATCH: Clarification or correction

## Enforcement

### Violation Handling

**Critical Severity**:
- Examples: Manual code without AI attempt, Hardcoded secrets committed, Security vulnerability deployed
- Action: Immediate rollback and remediation

**Major Severity**:
- Examples: Missing documentation, Skipped testing, Non-compliance with resource limits
- Action: Fix before phase gate

**Minor Severity**:
- Examples: Style guideline deviation, Incomplete AI interaction log
- Action: Fix before final review

### Audit Trail

**Required Records**:
- All git commits with descriptive messages
- AI tool command history
- Decision log with rationale
- Exception approvals
- Review meeting notes

**Retention**: Duration of project + 6 months

**Accessibility**: Available to all team members

## Continuous Improvement

### Feedback Loops

**Retrospectives**:
- Frequency: After each phase
- Focus: What worked well with AI tools? What didn't work as expected? How can prompts be improved? What should be added to constitution?

**Lessons Learned**:
- Documentation: LESSONS_LEARNED.md
- Template:
  ```
  ## Lesson: <title>
  **Phase:** <phase number>
  **Category:** <technical/process/tooling>
  **Description:** <what happened>
  **Impact:** <positive/negative>
  **Action Items:** <what to do differently>
  **Constitution Update:** <yes/no - if yes, reference amendment>
  ```

### Knowledge Transfer

**Artifacts**:
- This constitution document
- Documented AI prompts and patterns
- Troubleshooting playbooks
- Best practices catalog

**Sharing Mechanism**:
- Git repository (version controlled)
- Team wiki/documentation site
- Onboarding materials

## Governance

This constitution supersedes all other development practices and guidelines for Phase IV: Local Kubernetes Deployment. All implementation work, code reviews, and deployment decisions MUST verify compliance with these principles and constraints.

Complexity and deviations from these principles must be explicitly justified with evidence of necessity and documentation of simpler alternatives considered and rejected.

All team members are responsible for upholding these principles. Technical reviews at each phase gate will verify compliance.

**Version**: 1.0.0 | **Ratified**: 2025-12-24 | **Last Amended**: 2025-12-24

## Metadata

**Constitution Version**: 1.0.0
**Last Updated**: 2025-12-24
**Next Review**: After Phase IV completion
**Maintainers**: Phase IV Team

### Revision History

- **Version 1.0.0** (2025-12-24)
  - Changes: Initial constitution created for Phase IV: Local Kubernetes Deployment
  - Author: SpecifyPlus System
  - Scope: Established all core principles, architectural constraints, security requirements, quality gates, workflow phases, success metrics, and governance framework
