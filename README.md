# Bevy mod prefabs

Editor-ready prefabs for bevy.

```rust
trait PrefabQuery: Serialize {
    type Query: WorldQuery;

    fn from_query(item: QueryItem<Self::Query>) -> Self;
}
trait PrefabSpawn: Deserialize {
    fn spawn(&self, cmds: &mut PrefabEntityCommands);
}
trait Prefab: PrefabQuery + PrefabSpawn {
    fn editor_icon(&self) -> Option<Handle<Image>> { None }
    fn custom_aabb(&self) -> Option<Aabb> { None }
    fn layer(&self) -> EditorLayer { EditorLayer::DEFAULT_PREFAB }
}

struct PrefabEntityCommands<'a> {
  inner: EntityCommands,
  prefabs: &'a [dyn Prefab],
}
impl PrefabEntityCommands {
  // Everything that EntityCommands implements, but
  // change some impls so that it refers to the `prefabs` array
}

struct Scene {
  world: Vec<dyn Prefab>,
  // To set/unset resources etc.
  extra: fn(&mut World),
}