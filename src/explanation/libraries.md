# Libraries for parallelism in Julia

\warn{This page is a work in progress}

```julia:table
#hideall
using UnPack

repos = split(
    """
    https://github.com/tro3/ThreadPools.jl
    https://github.com/mohamed82008/KissThreading.jl
    https://github.com/baggepinnen/ThreadTools.jl
    https://github.com/Jutho/Strided.jl
    https://github.com/Jutho/TensorOperations.jl
    https://github.com/mcabbott/Tullio.jl
    https://github.com/invenia/Parallelism.jl
    https://github.com/JuliaParallel/Dagger.jl
    https://github.com/tkf/ThreadsX.jl
    https://github.com/JuliaFolds/Transducers.jl
    https://github.com/JuliaFolds/FLoops.jl
    """,
    "\n",
    keepempty = false,
)
docs_urls = Dict(
    "KissThreading.jl" => nothing,
    "Strided.jl" => nothing,
    "ThreadTools" => nothing,
    "ThreadPools.jl" => "https://tro3.github.io/ThreadPools.jl/",
    "Tullio.jl" => nothing,
)
keywords_mapping = Dict(
    "TensorOperations.jl" => "threaded, GPU",
    "Tullio.jl" => "threaded, GPU",
    "Dagger.jl" => "distributed",
    "FLoops.jl" => "threaded, distributed",
    "Transducers.jl" => "threaded, distributed",
)

projects = map(repos) do repository
    m = match(r"github.com/([^/]+)/([^/]+)", repository)
    user = m[1]
    package = m[2]
    docs = get(docs_urls, package, "https://$user.github.io/$package/stable/")
    keywords = get(keywords_mapping, package, "threaded")
    return (
        package = package,
        user = user,
        repository = repository,
        docs = docs,
        keywords = keywords,
    )
end

sort!(projects; by = x -> x.package)

println("| Package | User/Org. | Repository | Documentation | |")
println("| --- | --- | --- | --- | --- |")
for project in projects
    @unpack package, user, repository, docs, keywords = project
    code_badge = "[![GitHub](https://img.shields.io/github/stars/$user/$package?style=social)][$package-code]"
    if docs === nothing
        docs_badge = ""
    else
        docs_badge = "[![Documentation](https://img.shields.io/badge/docs-$package-blue.svg)][$package-docs]"
    end
    href = lowercase(replace(package, "." => "-"))
    println(
        "| **[", package, "](#", href, ")**",
        " | ", "**`@", user, "`**",
        " | ", code_badge,
        " | ", docs_badge,
        " | ", keywords,
        " |",
    )
end
println()

for project in projects
    @unpack package, repository, docs = project
    println("[$package-code]: $repository")
    println("[$package-docs]: $docs")
end
```

\textoutput{table}

## Threaded

\label{threadpools-jl}
### ThreadPools.jl

A custom thread pool framework for separating out latency-critical
code (e.g., GUI) from throughput-oriented code.

\label{kissthreading-jl}
### KissThreading.jl

\label{threadtools-jl}
### ThreadTools.jl

\label{strided-jl}
### Strided.jl

Threaded broadcasting, mapping, and reduce.

\label{tensoroperations-jl}
### TensorOperations.jl

Indexing notation-based domain specific language and the execution
backends for multi-threading and GPU.

\label{tullio-jl}
### Tullio.jl

Indexing notation-based domain specific language and the execution
backends for multi-threading and GPU.  Automatically provide
derivatives.

\label{parallelism-jl}
### Parallelism.jl

\label{floops-jl}
### FLoops.jl

\label{threadsx-jl}
### ThreadsX.jl

\label{transducers-jl}
### Transducers.jl

## Distributed

\label{dagger-jl}
### Dagger.jl
