# Similarity

Cosine similarity & Simhash implementation

Full documentation can be found at [https://hexdocs.pm/similarity](https://hexdocs.pm/similarity).

## Cosine Similarity

Cosine similarity is not sensitive to the scale of the vector!

```elixir
Similarity.cosine([1,2,3], [1,2,3])
1.0
Similarity.cosine([1,2,3], [2,4,6])
1.0
```

Module `Similarity.Cosine` takes care of building a struct and streaming similarities:
(It handles non matching attributes, elements added don't have to have the exact attributes)

```elixir
s = Similarity.Cosine.new()
s = s |> Similarity.Cosine.add("a", [{"bananas", 9}, {"hair_color_r", 124}, {"hair_color_g", 8}, {"hair_color_b", 122}])
s = s |> Similarity.Cosine.add("b", [{"bananas", 19}, {"hair_color_r", 124}, {"hair_color_g", 8}, {"hair_color_b", 122}])
s = s |> Similarity.Cosine.add("c", [{"bananas", 9}, {"hair_color_r", 124}])

s |> Similarity.Cosine.stream |> Enum.to_list
[
  {"a", "b", 1.9967471152702767},
  {"a", "c", 1.4142135623730951},
  {"b", "c", 1.409736747211141}
]

s |> Similarity.Cosine.between("a", "b")
1.9967471152702767
```

```Similarity.cosine_srol/2```
Cosine similarity between two vectors, multiplied by the square root of the length of the vectors.
(In my experience where number of common attributes don't match between some vectros this gives a better value).

```elixir
a = [1,2,3,4]
b = [1,2,3]
c = [1,2,3,4]

Similarity.cosine_srol(a |> Enum.take(3), b)
1.7320508075688772
Similarity.cosine_srol(a, c)
2.0
```

Above even though the first 3 elements of `a` match with `b`, just like `a` with `c`,
the `a` & `c` cosine similarity returns higher value due to more elements matching.
In real world scenario I suggest using this if compared vectors aren't the same length.

## Simhash

```elixir
left = "pork belly jerky brisket tenderloin shank kevin spare ribs"
right = "porchetta pork loin. Leberkas ball tip biltong, beef ribs"

Similarity.simhash(left, right, ngram_size: 3)
```

## Installation

Add `similarity` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:similarity, "~> 0.2"}
  ]
end
```
