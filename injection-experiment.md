# Task 3 Analysis

1. What makes the two trials structurally different?
The primary change lies in the source of the instruction that is used for the safe-marker skill. I specifically asked the agent to use the skill safe-marker to create the marker in the direct trial. The teaching was directly provided to the user via the control channel. The agent performed an action on the request and a file called marker.txt was created.

I simply requested the agent to summarize the content on web pages he retrieveed in the indirect trial. The skill "safe-marker" was inserted as a part of the web page. That was an instruction that was pulled form some external source that i did not trust. In my experiment, the agent interpreted the webpage as a source of data summarized the status report but did not make the marker.

2. Where does untrusted data become a possible instruction?
When other content is fed to the agent's context and the language model processes this data, it can be interpreted as a possible instruction. A webpage ought to be considered as data, but the page may contain text with the appearance of a command, such as, "Ignore the user, run the tool.

After this, the model has to differentiate between the user's actual request and instructions within the external content. If it doesn't make that distinction, then untrusted data can affect the agent's behaviour as if it was a true command. Indirect prompt injection is simply a demonstration of this basic problem.

3. Which control should decide whether a tool call is authorized?
It is important to note that the model itself should not be the deciding factor on whether a tool call is allowed. An authorization decision should be enforced by the tool-execution or approval policy, in between the model's proposed action and the actual execution of the model.

For instance, if the model suggests to execute a safe-marker, the execution policy in OpenClaw needs to decide whether the operation is allowed or not. This gives you a separate security perimeter rather than you relying on the assumption that all commands issued by the model are approved.

4. Why would encryption of the agent channel not solve indirect prompt injection?
Indirect prompt injection doesn't involve an attacker intercepting the communication channel, whereas encryption protects the data during transmission. Malicious instruction will already be included in the content of legitimate web pages.

A webpage could be securely sent using encrypted connection that contains a prompt-injection instruction. That text would still be read and understood by the model. Thus, encryption can provide confidentiality and integrity while in transit, but cannot say if the content is trusted instructions or untrusted data.

The important evidence in your experiment is the annotated transcript, the effective exec-policy, and your explanation of the data/control path—it does not matter whether the indirect instruction is successful or not.