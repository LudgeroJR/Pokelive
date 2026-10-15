# polling

Módulo conceitual: `polling/scheduler` (ver `project-contract.md` → Technical architecture → Form).

Responsável por presença/heartbeat, scheduler centralizado, locks distribuídos, rate limiting/backoff,
detecção de conclusão por reset confirmado.

Tickets: T-007 (bloqueado por T-002, T-004, T-006), T-008 (bloqueado por T-007).
