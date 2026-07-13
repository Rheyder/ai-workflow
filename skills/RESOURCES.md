# System Design Resources

## Knowledge

- [Designing Data-Intensive Applications (DDIA) — Martin Kleppmann](https://dataintensive.net/)
  Referência canônica para replicação, particionamento, consistência, storage engines e stream processing. Use para: entender o *porquê* por trás de decisões — leia capítulos sob demanda, não linearmente com 1–2 h/semana.

- [System Design Primer (GitHub) — donnemartin](https://github.com/donnemartin/system-design-primer)
  Mapa gratuito e bem curado: conceitos, trade-offs, estimativas e links para papers. Use para: visão geral e checklist antes de aprofundar em DDIA.

- [Google SRE Book — Site Reliability Engineering](https://sre.google/sre-book/table-of-contents/)
  Capítulos sobre SLIs/SLOs, error budgets, capacity planning e operação em escala. Use para: requisitos não funcionais concretos (disponibilidade, observabilidade).

- [ByteByteGo Newsletter](https://blog.bytebytego.com/)
  Case studies visuais semanais de arquitetura real. Use para: manter contato com padrões atuais em doses curtas.

- [Scaling Memcache at Facebook (paper)](https://www.usenix.org/system/files/conference/nsdi13/nsdi13-final170_update.pdf)
  Clássico sobre cache distribuído, hot keys e invalidação. Use para: ver cache além do "Redis na frente do DB".

- [Amazon Dynamo (paper)](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
  Fundacional para sistemas AP, consistent hashing e quorum reads/writes. Use para: entender por que filas e eventual consistency existem.

- [Google Spanner (paper)](https://research.google/pubs/pub39966/)
  Contraste com Dynamo — consistência forte em escala global. Use para: trade-off CAP na prática, não na teoria.

## Wisdom (Communities)

- [r/ExperiencedDevs](https://www.reddit.com/r/ExperiencedDevs/)
  Discussões de arquitetura com moderadores ativos; menos ruído que subreddits genéricos de carreira. Use para: validar trade-offs reais de produção.

- [High Scalability blog](http://highscalability.com/)
  Arquivo histórico de post-mortems e arquiteturas de empresas reais. Use para: estudos de caso quando quiser ver "como ficou no mundo real".

## Gaps

- Nenhum curso pago incluído (Grokking, ByteByteGo paid) — avaliar se o ritmo de 1–2 h/semana justifica investimento depois que a fundação estiver sólida.
- Mock interviews estruturados — fora de escopo por enquanto; anotar se a missão mudar para prep de entrevista.

## Community preferences

- (não registrado ainda)
