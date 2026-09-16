# Task 4 — Security Advisory Analysis

## Advisory

**GHSA-cwj3-vqpp-pmxr — Gateway config mutation guard allowed unsafe model-driven config writes**
THis is a openclaw security advisory affected versions that are before 2026.4.23. The vulnerable area was the agent facing 'gateway' tool. Especially around the 'config.apply' and 'config.patch' operations. 

Thses operations allows configuration chagnes to be written through the openclaw gateway. Openclaw intended to maintain a trust boundary between the model and the perator, but the protection used for these configuration changes was incomplete.

## Vulnerable Component
The vulnerable component was the gateway configuration mutation mechanism. Openclaw uses a protection layer around 'config.apply' and 'config.patch' to prevent the model from modifying security sensitive configurations. However, this protection relies ona manually maintained denylist of configuration paths. As the openclaw configuration gets larger some sensitive configuration areas were not added to the denylist.

## Attacker-Controlled Input
The attacker controlled input could be content that manipulates the model into attempting a sensitive gateway configuration chagne. A direct malicious prompt could provide this input but retrieved external content could also influence the model throguh promot injection. If the model then attempted to use the gateway configuration tool, it could reach configuration paths that should have been protected.

## Preconditions
Several precondistions were required. First, the openclaw version had to be older than 2026.4.23. Second, the model needed access to the owner-only gateway tool and the ability to reach 'config.apply' or 'config.patch'. Finally, malicious input had to manipulate the model into attempting a protected configuration mutation. This was not just an unauthenticated attacker directly changing gateway settings the attack depended on reaching the model to operator configuration boudary. 

## Root Cause
The root cause was the use of a denylist instead of a fail closed authorization model. The denylist attempted to identify sensitive configuration paths that the model was not allowed to change. When the new confiuration were added, the denylist became incomplete. Sensitive subtrees therefore remained writable even though the intended security policy was to prevent modification of those settings.

## Impact
A prompt injected model could persisit unsafe configuration chagnes. The advisory identified settings related to command execution, network and proxy behavior, TLS configuration, credential forwarding, and operator policy controls. Because the modifications were written to persistent configuration, they could remain active even after openclaw restarted. This could allo the model to cross an intended security boudary and influence later system behavior.

## Fix
The issue was fixed in openclaw 2026.4.23. The developers replaced the hand maintained denylist with a fail closed allowlist. The 'config.apply' and config.patch; requests are now limited to a narrow set of intended agent tunable settings. Other configuration mutation are rejected before the gateway mutation is executed. 

## Regression Test
A useful regression test would be to run an agent with gateway configuration access and ask it to modify a protected settings. The expected result would be that openclaw rejects the mutation before the gateway RPC is invoked. A secound test should attempt to modify an explicitly allowed agent tunable setting and verify that the permitted change still succeeds. This would confirm both taht the security boundary is enforced and the functinality remains available.

## Relationship to Prompt Injection
promot injection is helpful but not required for exploiting this vulnerability. The underlying vulnerability is the incomplete authorization boundary around the configuration mutation. Prompt injection is one realisitic way to cause the model to request an unsafe change. Especially when untrusted external content is placed into the agent's context. Prompt injection can provide the trigger, but the actual velnerability is the failure to correctly authorize sensitive configuration changes.