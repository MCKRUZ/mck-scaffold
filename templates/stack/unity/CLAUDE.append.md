### Stack: Unity / game development

#### Project structure

- **Assembly Definitions for every logical module.** Never dump scripts into the default assembly — compile times and dependency clarity both degrade fast.
- **ScriptableObjects for data-driven design** — items, abilities, enemy stats, config blobs. They live in `Assets/Data/`.
- **Prefab-first workflow** — compose behavior into prefabs, assemble prefabs into scenes. Scenes are the integration layer, not the source of truth.
- **Component composition over inheritance hierarchies.** Don't build deep `MonoBehaviour` inheritance trees.

#### C# game code style

- **MonoBehaviour for Unity lifecycle; pure C# for game logic.** Pure logic should be testable without Unity loaded — write it that way from the start.
- **`[SerializeField] private` over `public` fields.** Inspector access without breaking encapsulation.
- **Avoid `Find*` methods at runtime.** Inject references via the Inspector or a service locator. `GameObject.Find` is a tax on every frame and a refactoring trap.
- **Coroutines for simple sequences; UniTask or async/await for complex async flows.** Don't mix — pick one per system.
- **Object pooling for frequently spawned objects** — projectiles, effects, UI elements. Allocate-then-recycle, never allocate-then-destroy in a hot path.
- **Events / delegates for decoupling systems** — not direct references between managers. `PlayerHealth.OnDied` invokes; whoever cares subscribes.
- **State machines for entity behavior** — enum + switch is fine; dedicated `StateMachine<T>` class for complex actors.

#### Performance

- **Profile before optimizing.** Unity Profiler for CPU/GPU; Nsight Systems for cross-process bottlenecks.
- **Draw call batching:** static batching → dynamic batching → GPU instancing → SRP Batcher. Pick per asset class.
- **LOD groups for 3D assets**, texture streaming for large texture sets.
- **Avoid per-frame allocations.** Cache arrays. Use `NativeArray` + Burst for hot paths.
- **Frame budget:** 16.67 ms (60 fps) or 33.33 ms (30 fps mobile). Measure against this, not against "feels smooth."

#### Shader development

- ShaderLab + HLSL (not GLSL). Unity-native pipeline.
- Shader Graph for artist-facing shaders; hand-written HLSL for performance-critical paths.
- Always provide fallback subshaders for lower hardware.
- Minimize texture samples. Prefer half precision where possible.
- Test shaders on target hardware — Shader Graph preview ≠ final render.

#### NVIDIA integration

- **DLSS**: integrate via Unity Package Manager NVIDIA package, not manual SDK setup.
- **PhysX**: already Unity's physics — tune via Physics settings, not raw PhysX API.
- **Nsight**: Nsight Systems for CPU/GPU bottleneck identification; Nsight Graphics for shader debugging.
- **RTX Kit components**: reference from `D:\NVIDIA\RTX-Kit\` when building custom rendering.

#### Testing

- **Pure game logic** (damage calc, inventory, turn order): standard xUnit / NUnit tests, no Unity dependency.
- **MonoBehaviour integration**: Unity Test Framework in Play Mode.
- **Scene tests**: validate scenes load, required objects exist, no null refs after `Awake` and `Start`.
- **Performance tests**: Unity Performance Testing package for regression detection.

#### Build hygiene

- Editor scripts in `Editor/` folders only (won't ship to runtime).
- Avoid hardcoded asset paths — use `[SerializeField]` references.
- Pre-build: verify no missing references in scenes, no unused assets in resources.
