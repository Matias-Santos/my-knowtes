---
id: "a0c2b16c-8420-4354-a264-67a8958db9ac"
title: "useState: Guardar Estado en Componentes React"
tl_dr: "useState se usa para guardar y gestionar el estado interno de los componentes de React."
created_at: "2026-09-03T11:35:30.011326+00:00"
updated_at: "2026-09-03T11:35:30.011341+00:00"
source: "claude-sonnet-4-6"
---

# useState: Guardar Estado en Componentes React

El hook `useState` es el mecanismo principal para guardar estado dentro de un componente de React. Permite que el componente "recuerde" información entre renders, como un valor de entrada, un contador, o cualquier dato que pueda cambiar con el tiempo.

**Uso básico:**
```js
const [valor, setValor] = useState(valorInicial);
```

- `valor` — el estado actual
- `setValor` — la función para actualizarlo
- Cada vez que el estado cambia, React re-renderiza el componente

## Insight

useState es el punto de entrada al estado en React, pero es importante entender cuándo usarlo con actualizaciones funcionales (e.g., `setValor(v => v + 1)`) para evitar problemas de estado stale, especialmente dentro de callbacks o efectos.
