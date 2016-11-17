# Dialogues, part I

## Simplest functional requirements, Socratic style

*Trying something new.*

### Act I

*[Mercutio and Capulet enter, stage right]*

Mercutio: A question. What's the simplest thing that produces value?

Capulet: Assuming you've got something to build, a build stack.

**M:** Okay, let's start there.

**M:** How to make a build stack as simply as possible?

**C:** Jenkins. Master and node all on one box. Run it locally on Vagrant.

**M:** Now you're just guessing. Maybe AWS is simpler.

**C:** Maybe so, but AWS requires Internet connectivity.

**M:** Fair enough.

**C:** On the other hand, what does Vagrant give you? Something akin to an EC2 instance, and that's about it. No S3, no RDS or DynamoDB, no ELBs, no autoscaling, no security groups, no VPCs. I could go on. Almost none of the good stuff.

**M:** You've changed your own mind!

**C:** Maybe. But it's hard to say I've changed my mind, when I've hardly made up my mind at this point!

**M:** It strikes me that maybe asking how to make a stack as simply as possible may not be quite the right question. What about instead asking **what's the simplest thing that meets a set of core functional requirements?**

**C:** Asking for functional requirements is a slippery slope. Where do you draw the line?

**M:** I don't know. I guess you get that from experience.

**C:** Okay, let's set some ground rules: AWS is in, Vagrant is out. I could say "stacks that are expandable are in, those that are not are out", but that's too abstract. Functional requirements should be concrete. Maybe we could create a list of preferred tools. AWS would surely be on that list.

**M:** Sounds good. But how would that apply in the other direction? What would you say about an application that doesn't need a database today? Is there some way to add that as a requirement later?

**C:** Probably, but adding a database and the provisioning source code feels kind of YAGNI right now.

**M:** Agreed. Let's table that for now.

**C:** Okay, we need SP/CM/CI/CD. Let's break them down one at a time. Server provisioning.

**M:** Yes. Hmm. That's Terraform or CloudFormation. But why spend any more time on CF?

**C:** Agreed. Terraform is a rich vein. Let's go with that. On to configuration management.

**M:** That's Ansible or Chef. Gotta choose one.

**C:** Since Chef either requires a server or has to run in solo/zero mode, I'm inclined towards Ansible. On the other hand, everyone seems to use Chef. On the other OTHER hand, I'm not aiming to appease everyone. I'm aiming first to appease myself.

**M:** Ansible then?

**C:** Yes.

**M:** CI. DroneCI or Jenkins?

**C:** I don't care for Jenkins. Also, what about Distelli?

**M:** Same comments as earlier. Jenkins is everywhere. But I'm not looking to attract companies that think it's good enough. It's kind of bolted together, and not my cup of tea. (And I say that as someone who's a co-organizer of the Seattle JAM! But that's more about the Jenkins community than Jenkins itself.) So let's go with DroneCI for now.

{{ Note: this is kind of an arbitrary decision. }}

But I also want to take another look at SaaS Distelli. The major differentiator there is that it's hosted. That was a deal breaker for COFI, but maybe it won't be for other companies.

**C:** Also, Distelli is starting work on Kubernetes support, which is the hotness.

**M:** Flock yes. Okay, we've got SP, CM and CI covered. What about CD?

**C:** Continuous delivery? Or continuous deployment?

**M:** What's the difference?

**C:** I should know, but don't know offhand. It's documented in **that one blog post** (TODO which?). Basically it comes down to: are you deploying every time, or just making sure the code (all of it, including IaSC) is ready to deploy every time?

**M:** That brings up an interesting point. There's no way you can call something ready to deploy without automated testing.

**C:** That's true, but off-topic for now. We'll have to come back to it later. Right now, we need to decide on a deployment strategy. It's where things get ... complicated. On the one hand, there's *traditional* server-based deployments, using rsync or whatever. On the other hand, there are container based deployments, but that's a whole other ball o' wax. Finally, there are now serverless ... can you even call them deployments at that point?

**M:** Holy shirt.

**C:** Yup. We have to assume a containerized world. We also have to assume a serverless world. But for now, let's stick with server based deployments, and add the other two to a roadmap document. It's pretty clear that our single solution won't fill the bill. But that shouldn't prevent us from getting started.

**M:** Good call. So we're going with server based deployments, which means we need to create build and destination stacks.

**C:** Right. That brings us back to DroneCI. Why is it okay to use Docker for builds, but not for deployments? In other words CI yes, CD no. That makes no sense.

**M:** Right. So let's stay consistent and take Docker and other containerization tools off the table for now. That leaves us with Jenkins or something else, like Bamboo.

**C:** Jenkins is bad enough. I'd rather not even consider the alternatives. Remember, this is the starting state, not where we ultimately want to be.

**M:** True dat. Okay, so Jenkins?

**C:** Jenkins.

**M:** Are we ready to recap?

**C:** I think so.

{{ Actually, no we're not. We never finished the conversation about a continuous delivery tool or tools. }}

**M:** Server provisioning will be Terraform. Also, when we say server provisioning, what we really mean is stack provisioning. So I'd like to start using that terminology instead.

**C:** Yup, sounds good. Next is configuration management. We talked about Chef and Ansible, but decided on Ansible because ... actually I don't have a good reason, aside from thinking that Chef is overly complicated.

**M:** Speaking of Chef and its ilk, we'll need to look into a tool for secrets management.

**C:** Shirt, you're right. That's a subset of configuration management, but would be helpful spread across all areas of SP/CM/CI/CD, like peanut butter. I can see the diagram in my mind's eye. So where does that leave us?

**M:** We need to decide where secrets management fits in.

**C:** This set of core functional requirements keeps growing.

**M:** It does. How would you have handled this back in the XP (eXtreme Programming) days?

**C:** We would spike a few solutions, and demo them. Mainly we'd create something that delivers some quantum of value, without (much) regard for refactoring or making it look nice. The point is to learn about the systems you're thinking about creating.

**M:** Nowadays, the only thing we have like that is the sprint zero. We should identify the epics first, then break them down into stories. Finally we can start to prioritize the work.

**C:** But that's the problem. How do you prioritize the work, shirt, how do you write the stories to begin with, without some idea what works and what doesn't?

**M:** We write the stories for those parts we can. Then write the sprint zero stories. Prioritize the sprint zero stories first. Do the sprint zero stories. Finally, go back and write the rest of the stories based on the results of those stories.

**C:** Ugh.

**M:** It's an iterative process.

**C:** Still sounds like a lot of work.

**M:** It is, but the biggest problem with having a lot of work is getting overwhelmed and losing focus. Or spinning and not actually **finishing** anything.

**C:** I hear ya!

**M:** This is one area where Trello may not be a lot of help. We'd be better off with a "real" agile tool, like Jira. Which reminds me, I need to ask the technology folks from PNNL if they do agile at all.

**C:** Let's stay on topic. What if we first summarize our decisions so far. I don't think there's really that much left to decide via sprint zeroes.

**M:** Cool cool.

*[Both work together to create the list]*

* Stack provisioning - build stack: Terraform
* Stack provisioning - deployment stack: Terraform
* Configuration management: Ansible
  * **NOTE:** configuration management only, no deployments!
* Continuous integration: TBD
  * DroneCI or Jenkins
* Continuous deployment: TBD
  * Several possibilities here: *Ansible*, *Distelli*, *Jenkins Pipeline*, *DroneCI*
  * **NOTE:** Setting continuous deployment aside for now
* Secrets management: TBD
  * HashiCorp Vault or #2


**M:** Wow, that's quite a list. Only Terraform for the stacks and Ansible for configuration management are settled. We've got to decide on tools for CI, CD (deployment) and secrets management.

**C:** Ansible can do deployments. Want to slot it in for CD?

**M:** I guess so. I'm also thinking again about Distelli, but since we're already using Ansible, it's probably better to stick with it for now. For secrets management, I'm inclined towards HashiCorp Vault for a similar reason. We're already using Terraform, so why not continue with a tool that should have good integration with it?

**C:** Sounds good to me. That leaves only CI with DroneCI or Jenkins. And honestly, if we go with Jenkins for CI, we should consider it for CD as well.

**M:** Hmm. Probably so.

{{ Updated list. }}

* Stack provisioning - build stack: Terraform
* Stack provisioning - deployment stack: Terraform
* Configuration management: Ansible
* Continuous integration: TBD
  * DroneCI (possibly paired with DroneCI or Ansible for CD)
  * Jenkins (paired with Jenkins for CD)
* Continuous deployment: TBD
  * DroneCI
  * Ansible
  * Jenkins
* Secrets management: HashiCorp Vault


**M:** It seems to me that Chef, Jenkins and CloudFormation are all out based on preference.

**C:** Yes, that's arguably true. But even after removing them, there are still too many choices. So I'm okay with it.

**M:** One thing that's becoming obvious is that we need to research DroneCI.

**C:** Yep, and Ansible for deployments too.

**M:** Based on a quick scan of the DroneCI documentation, I'm going back to the original idea that we should *"take Docker and other containerization tools off the table for now."* Containerization in general, and Docker (or Kubernetes) in particular are big topics, and I don't want to get spread too thin. Would you be okay if we use Jenkins for CI? We can then use either Jenkins Pipeline, or Ansible for CD.

**C:** Honestly, I'm ready to make some decisions and get going on writing stories. So yes, let's go with Jenkins for CI and move on. We can table the CD question for now, and maybe spike a solution with both. Doing that for one area is way more manageable than three!

**M:** Sounds like a plan! Let's revise our list again.

{{ Updated list. }}

* Stack provisioning - build stack: Terraform
* Stack provisioning - deployment stack: Terraform
* Configuration management: Ansible
* Continuous integration: Jenkins
* Continuous deployment: Ansible or Jenkins
* Secrets management: HashiCorp Vault


**C:** I hesitate to say it, but we've not considered testing tools at all yet.

**M:** Nope. And I'm too fried right now to even think about it. Let's talk about it tomorrow morning.

*Mercutio and Capulet exit stage right*



### Act II

#### Scene I

*[Both are back at work the next day]*

**M:** Good morning, Capulet. I've started reading _Ansible for DevOps_. At the top of page ix, it says Ansible does three things in one. If I'm reading it right, the three things are **configuration management**, **server deployment** and **task execution**.

* [Ansible Documentation](https://docs.ansible.com/ansible).
* [Ansible Glossary!](https://docs.ansible.com/ansible/glossary.html)  Hot sauce!
* [Ansible for DevOps Errata](https://www.ansiblefordevops.com/errata)

Also, I've just learned that Vagrant can, in fact, manage more than one server at a time. It can also provision a database! This is important, because the book uses Vagrant as a local development environment, which is (IMO) it's main benefit. Super.

**C:** That's good stuff! No question.

**M:** But ... ?

**C:** But we're going broad when I think we should go deep. Last week I passed the AWS Certified Developer Associate exam.

**M:** Well done!

**C:** Many thanks, Mercutio! I don't consider it a really big accomplishment. But I got there by focusing on one thing at a time. More to the point, I set myself the goal of **completing** one thing at a time.

**M:** And it paid off.

**C:** It did indeed.

**M:** I see what you're saying, but this is coming somewhat out of left field. It's not what we've been talking about, which is to my way of thinking, also about completing one thing. In this case, that one thing is coming up with an initial cut at a preferred tech stack.

**C:** Yes, but now you're off reading about Ansible.

**M:** I've got to learn a bit about it before deciding if or how it fits into our plans.

**C:** [With a whiff of righteousness.] I'd just caution you not to lose sight of the goal.

**M:** [With a bit of coldness.] Point taken. I'm going for coffee, will be back in 20 minutes. Care to join me?

*[Both walk to Zeitgeist]*


#### Scene II

[They've talked about their minor irritation with each other, and all is well. No more will be said about it right now.]

**M:** Back to it. Do we *now* have enough info to make some decisions? We've identified many of our tools, and have also identified some gaps. Here they are again:

{{ Updated list from yesterday. }}

* Stack provisioning - build stack: Terraform
* Stack provisioning - deployment stack: Terraform
* Configuration management: Ansible
* Continuous integration: Jenkins
* Continuous deployment: Ansible or Jenkins
* Secrets management: HashiCorp Vault


**C:** If we add in validation and testing, that list becomes twice as long! That's assuming and/or deciding that we need testing everywhere along the way.

**M:** Why do we need testing everywhere? I mean, is validation and testing everywhere really worth the effort?

**C:** I suspect it is, but let's go back to first principles and build from there. Do we agree that testing source code at different "levels" [Capulet, muttering to himself: "need a better word for this"] is appropriate and worth the effort?

**M:** Yes, it's well understood by now that without unit testing, for instance, it's more difficult to refactor your code or add features without risking uncaught regression. XP says it gives you courage. That's always stuck with me.

**C:** What about functional testing? What about integration testing?

**M:** Those are, to use your word, different levels. They bring different values. Functional testing is typically part of the acceptance criteria for a story.

**C:** So it is. But isn't that begging the question? Just because we're doing it now doesn't mean we should be doing it at all.

**M:** An appeal for rigor. Excellent. IMO, we do functional testing for at least two complementary reasons. First, we want to ensure that things work as expected. Second, we ensure that things that should not work fail without errors.

**C:** You're introducing bias. We're ensuring things work as expected, but who's expectation?

**M:** We want to ensure things work as specified in the functional requirements documentation.

**C:** Not to beat a dead horse, but that's another bias. You're assuming that we've created a functional requirements document.

**M:** A modern analog, but poor substitute might be the *definition of done* for the story in which the feature is implemented. And I know what you're going to say next: now I'm assuming you're using stories and working in an agile methodology. Let's clarify some base assumptions / expectations.

**C:** Down the rabbit hole with Alice ...

**M:** Not at all. We're basically putting down the foundations for what's to come. Let's break for a few minutes then get into it. Once we've agreed to terms and expectations, we'll have a solid core from which to work.


#### Scene III

**M:** Okay, I took us in this direction, so I'll start. We need some way of managing inputs. How do we know what to work on?

I propose we assume we're using Agile with at least the "core" components and ceremonies.

**C:** Wait, what? There are no core components or ceremonies. That's another bias.

**M:** Oh shirt, I think you're right. In my mind, the core components are:

*[Mercutio goes to the whiteboard and starts writing]*

* a backlog owner (a person) who sets priorities for the team
* a scrum master (a person) who reviews the work in progress, and facilitates the ceremonies
* a product backlog in some trusted system, like Jira
* a set of groomed stories in priority order
* the notion of a sprint, including length (maybe two weeks) and a calendar
* [optional but encouraged] a burn down chart for the current sprint

Then the core ceremonies are:

* a poker pointing session where groomed stories are evaluated for level of effort
* a quick daily standup where everyone says what they did the day before, what they're doing today, and if they have any impediments
* [optional] design discussions among the team
* [optional but encouraged] 30% and 90% demos for the stakeholders
* a sprint acceptance meeting
* a sprint retrospective, where developers can talk freely about what worked and what didn't work. The scrum master may capture notes, and those notes may be shared with the backlog owner and/or product owner. *[attendees: sprint implementers and scrum master/facilitator]*

Some additional notes:

* Stories on the happy path move from *new* to *grooming* to *ready (groomed)* to *in progress* to *done* to *accepted*.
* Standup is **not** a status meeting. It's for the developers to communicate with one another and with the scrum master. The product owner may be there, but many teams would prefer they not be. They'll see the results at the demo.
* The backlog owner is typically responsible for working with a product team or someone representing end users.
  * This can get tricky because developers may feel like they're not part of the conversation about evolving the product. So it's really important to have an open discussion and make sure people are heard. Ultimately, however, the product owner and the backlog owner need to come to agreement.


**TODO stick this in a box**

BEGIN Important note:

We need a working agreement among all stakeholders.

It's implied here, and I want to make it explicit. Different people are involved in different parts of the process. For example, the product owner and the backlog owner will have to meet to decide on product development priorities at a high level. Then the backlog owner and the scrum master will probably meet to write stories. They may groom them at the same time.

*A note on story grooming* Ultimately, this is the responsibility of the backlog owner. They may have to consult with the developers or stakeholders ...

*[Capulet interrupts: stakeholders? Mercutio provides a definition. TODO: define this somewhere. Holy shirt, rabbit hole is right. But a glossary is a **tremendously** good idea.]*

As I was saying, they may have to consult with the developers or stakeholders to understand exactly what is being requested, **and** whether the request is feasible. Once it's within range, the backlog owner should finish grooming the story.

But I'm getting off topic. The point is that ...

Working agreements could be something like:

*aaand the first thing I do is write definitions. WTF? :)*

* <DEF feature team> consists of product owner, backlog owner, scrum master, developers (&& QA I guess, if you roll that way)
* <DEF sprint team> is defined as everyone except the product owner

* AGREEMENT: product owner and backlog owner are required at product prioritization meetings; scrum master is optional
* AGREEMENT: backlog owner and scrum master are required at backlog grooming meeting; sprint team developers are optional
* AGREEMENT: product owner and sprint team developers may be called upon to fill in gaps in backlog owner's understanding, to help with story grooming and maturation. *[Capulet: maturation? Mercutio provides a definition.]*
* AGREEMENT: Stories should come to poker pointing fully groomed. All maturation should be completed before the meeting, or the story will be de-prioritized until it's properly groomed.
  * There are two sides to this. On one hand, the backlog owner could argue that the story pointing session is the best time to mature the story with the whole sprint team present. On the other hand, the developers could (rightly, in my view) argue that a poker planning session is not the time for story maturation.
  *
* AGREEMENT: everyone on the sprint team attends poker planning
* AGREEMENT:
* AGREEMENT:
* AGREEMENT:
* AGREEMENT: 

END Important note


At that point the stories should be groomed.

After that, the stories are either groomed and ready to be pointed, or the backlog owner and scrum master will want to meet with some or all of the team to continue grooming them. This is a slippery slope, though, because the

* [not typically ] a written working agreement



**C:** That's a loaded question, but I'll take the bait. Yes, of course it is.

* Stack provisioning - build stack: Terraform
  * Validation and testing: **TODO**
* Stack provisioning - deployment stack: Terraform
  * Validation and testing: **TODO**
* Configuration management: Ansible
  * Validation and testing: **TODO**
* Continuous integration: Jenkins
  * Validation and testing: **TODO**
* Continuous deployment: Ansible or Jenkins
  * Validation and testing: **TODO**
* Secrets management: HashiCorp Vault
