<p align="center">
  <img src="images/osticket-logo-banner.png" alt="osTicket logo" width="100%" />
</p>

# BUIKE-HELPDESK: Initial Configuration Walkthrough

**Phase 2 of the Ashgrove Clinic osTicket engagement, Bridgeway Technology**

This phase picks up right after the base install: osTicket and all its dependencies are already running on the VM. Here we configure the actual help desk, roles, SLAs, departments, teams, agents, and help topics, all scoped around a fictional MSP engagement where Bridgeway Technology has been hired to stand up ticketing for Ashgrove Clinic.

---

## Why This Phase Exists

A fresh osTicket install is just a shell. Nothing in it reflects how a real clinic IT department actually runs: who has access to what, how fast a critical outage needs a response versus a routine password reset, and which category a ticket falls into before it ever reaches an agent.

This phase builds that structure out. The goal was to configure osTicket the way an MSP tech would on day one of a real engagement: set up admin access first, then SLAs, then departments, then the people, then the categories tickets get filed under.

---

## Environment

| Component | Detail |
|---|---|
| Platform | Microsoft Azure, Windows Server VM |
| Application | osTicket v1.15.8 |
| Admin persona | chibuike (Jordan Reyes, jreyes) |
| Fictional MSP | Bridgeway Technology |
| Fictional client | Ashgrove Clinic |
| Project name | BUIKE-HELPDESK |

---

## What Got Configured

### 1. Roles

**Path:** Admin Panel > Agents > Roles

![Agents menu with Roles highlighted](images/01-agents-menu-dropdown.png)
*The Agents dropdown in the admin panel, showing Agents, Teams, Roles, and Departments as the four sub-tabs used throughout this phase.*

Before adding anything, the built-in roles were reviewed for a baseline: All Access, Expanded Access, Limited Access, Supreme Admin, and View only.

![Roles list before the new role was added](images/02-roles-list-before.png)
*The Roles list showing the five built-in roles that ship with osTicket, before IT Director was created.*

A custom role, IT Director, was created for the person overseeing Clinical IT Support.

![Add New Role, Definition tab, name set to IT Director](images/03-add-role-definition-tab.png)
*The Definition tab of the Add New Role screen with the role name entered as IT Director.*

![Add New Role, Permissions tab, all ticket permissions checked](images/04-add-role-permissions-tab.png)
*The Permissions tab under Tickets, with every permission checked (Assign, Close, Create, Delete, Edit, Edit Thread, Link, Mark as Answered, Merge, Post Reply, Refer, Release, Transfer) to give the role full ticket access.*

![Roles list confirming IT Director was added](images/05-roles-list-it-director-added.png)
*The Roles list after saving, with a green confirmation banner and IT Director now showing as an active role alongside the built-in ones.*

Permissions in osTicket are granted per department, not globally, so the same agent can hold different roles in different departments if they need extended access to more than one.

### 2. Departments

**Path:** Admin Panel > Agents > Departments

Departments represent the IT support org, not the hospital units requesting help. They control who owns and can see a given ticket.

![Departments list before any new departments were added](images/06-departments-list-before.png)
*The Departments list showing only the default Support department that ships with osTicket.*

A System Admins department was created and configured underneath Support, with a manager assigned and the schedule set to 24/7.

![Update Department screen for System Admins](images/07-update-department-system-admins.png)
*The Update Department screen for System Admins, showing the parent set to Support, status Active, type Public, schedule 24/7, and okerulu, chibuike assigned as manager.*

A dedicated department email address was also set up for Clinical IT Support so tickets routed there would send and receive under a distinct identity.

![Add New Email Address form for Clinical IT Support](images/08-add-email-clinical-it-support.png)
*The Add New Email Address form with the address set to clinic@ashgroveclinic.com, name Clinical IT Support, department Support/Clinical IT Support, and priority set to High.*

![Email Addresses list confirming Clinical IT Support was added](images/09-email-list-clinical-it-support-added.png)
*The Email Addresses list with a confirmation banner and the new Clinical IT Support address now showing High priority alongside the default addresses.*

### 3. Teams

**Path:** Admin Panel > Agents > Teams

Teams group agents across departments, useful when a group needs shared visibility into a category of issue regardless of their primary department. A Clinical Systems Support team was created to represent Clinical IT Support staff plus help desk agents who jointly handle tickets tied to patient-facing systems.

![Update Team screen for Clinical Systems Support](images/10-update-team-clinical-systems-support.png)
*The Update Team screen for Clinical Systems Support, showing status Active and no team lead assigned yet.*

### 4. Agents

**Path:** Admin Panel > Agents > Add New

Two agent accounts were created: Devon Ricci as IT Director over Clinical IT Support, and Marcus Bell as a View Only agent under the default Support department.

![Add New Agent, Access tab, department dropdown open](images/11-add-agent-access-tab-department.png)
*The Access tab of the Add New Agent screen with the Primary Department dropdown open, showing Support, Support/Clinical IT Support, and Support/System Admins as options, with Clinical IT Support selected for Devon.*

![Add New Agent, Teams tab, Clinical Systems Support selected](images/12-add-agent-teams-tab.png)
*The Teams tab of the Add New Agent screen with Clinical Systems Support added as the agent's assigned team.*

After each agent was created, their password was set separately through the agent's profile.

![Set Agent Password modal](images/13-set-agent-password-modal.png)
*The Set Agent Password modal, showing password and confirm password fields along with the option to require a password change at next login.*

![Final agents list with Devon Ricci and Marcus Bell added](images/14-agents-list-final-devon-marcus.png)
*The Agents list showing all three accounts: chibuike okerulu under Support, Devon Ricci under Clinical IT Support, and Marcus Bell under Support.*

### 5. SLAs

**Path:** Admin Panel > Manage > SLA

Three SLA tiers were built around patient impact rather than generic severity labels, all running against the default 24/7 schedule created for this phase.

| SLA | Grace Period | Use Case |
|---|---|---|
| Sev A | 1 hour | Critical outages affecting patient care systems |
| Sev B | 4 hours | Significant disruption, not directly patient-facing |
| Sev C | 8 hours | Routine requests, password resets, general support |

![SLA plan list showing Default, Sev A, Sev B, and Sev C](images/15-sla-list-sev-a-b-c.png)
*The Service Level Agreements list showing all four plans: Default SLA at 18 hours, Sev A at 1 hour, Sev B at 4 hours, and Sev C at 8 hours, all active.*

![Update SLA Plan screen for Sev A](images/16-update-sla-plan-sev-a.png)
*The Update SLA Plan screen for Sev A, showing a 1 hour grace period against a 24/7 schedule.*

### 6. Help Topics

**Path:** Admin Panel > Manage > Help Topics

Help topics are the categories hospital staff choose when submitting a ticket, and they can auto-route tickets to a department, priority, and SLA plan.

![Help Topics list before the new topics were added](images/17-help-topics-list-before.png)
*The Help Topics list showing only the three default topics (Feedback, General Inquiry, Report a Problem), all routed to Support.*

A Critical System Outage topic was added first, since it needed the fastest routing of the group.

![Add New Help Topic screen for Critical System Outage](images/18-add-help-topic-critical-outage.png)
*The Help Topic Information tab with the topic name set to Critical System Outage, status Active, type Public, and no parent topic.*

![New ticket options for Critical System Outage](images/19-critical-outage-ticket-options.png)
*The New ticket options tab showing the topic routed to Support/System Admins, priority set to High, and the Sev A (1 hour) SLA plan applied.*

A Workstation / PC Issue topic was added underneath Report a Problem as a sub-topic, since it's a specific flavor of a broader complaint category.

![Add New Help Topic screen for Workstation / PC Issue](images/20-add-help-topic-workstation-pc.png)
*The Help Topic Information tab with the topic name set to Workstation / PC Issue and Report a Problem selected as the parent topic.*

![New ticket options for Workstation / PC Issue](images/21-workstation-pc-ticket-options.png)
*The New ticket options tab showing the topic routed to Support/System Admins, priority High, and the Sev A SLA plan applied.*

An Equipment Request topic rounded out the set, routed at a lower urgency since it isn't tied to an active outage.

![Add New Help Topic screen for Equipment Request](images/22-add-help-topic-equipment-request.png)
*The Help Topic Information tab with the topic name set to Equipment Request, status Active, type Public, and no parent topic.*

![New ticket options for Equipment Request](images/23-equipment-request-ticket-options.png)
*The New ticket options tab showing the topic routed to Support/System Admins, priority Normal, and the Sev C (8 hour) SLA plan applied.*

![Final help topics list](images/24-help-topics-list-final.png)
*The completed Help Topics list showing Feedback, General Inquiry, Report a Problem, and Report a Problem / Access Issue, each with its assigned department and priority.*

---

## Build Phases

1. Reviewed built-in roles, then built a custom IT Director role with full ticket permissions
2. Created the System Admins department and a dedicated Clinical IT Support email address
3. Created the Clinical Systems Support team for cross-department visibility
4. Created Devon Ricci (IT Director, Clinical IT Support) and Marcus Bell (View Only, Support), setting passwords separately after account creation
5. Built out three SLA tiers (Sev A, Sev B, Sev C) framed around patient impact
6. Added Critical System Outage, Workstation / PC Issue, and Equipment Request help topics, each routed to a department, priority, and SLA plan

---

## Recap

By the end of this phase:

- An IT Director role exists with full ticket permissions
- A System Admins department and a Clinical IT Support email address are configured
- A Clinical Systems Support team exists for cross-department ticket visibility
- Two agents are active: Devon Ricci (IT Director) and Marcus Bell (View Only)
- Three SLA tiers are live: Sev A (1hr), Sev B (4hr), and Sev C (8hr), all on a 24/7 schedule
- New help topics route tickets by patient impact: Critical System Outage and Workstation / PC Issue at Sev A, Equipment Request at Sev C

**Next step:** creating and working mock tickets, submitting as an end user, then triaging, assigning, and resolving as an agent.

---

## Notes for Ashgrove Portfolio Framing

When explaining this build to a hiring manager, this phase demonstrates:

- **RBAC design**, building a custom role scoped deliberately, with the understanding that osTicket permissions apply per department rather than globally
- **Ticket routing logic**, the distinction between department (primary ownership) and team (cross-department visibility for a category of issue)
- **SLA-driven prioritization**, tiered severity levels tied to patient-impact framing rather than generic labels like low, medium, and high
- **Help desk taxonomy**, topic design that reflects a realistic mix of ticket types a clinic IT department would actually see, each one routed to the right department, priority, and SLA on creation

---

*Bridgeway Technology, Ashgrove Clinic engagement*
