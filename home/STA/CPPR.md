### CPPR
---
CPPR seems like a normal technology. Butm when the CPPR happen in a half cycle check, there are some difference.
Half cycle check is the negetive trigger with positive tigger, see in STA.

in case of half cycle paths, CRPR calculation will be different because the rise & fall delays of cells in the common path are different.
this delay difference cant be removed. but tool will remove the difference which is smaller in the below two cases.
1) difference with rise delay in common path
2) difference with fall delay in common path
