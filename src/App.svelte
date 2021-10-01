<script>
  let todos = [];
  let todo = { id: "", texto: "", estado: false };
  
 if(localStorage.getItem("todos")){
	 todos = JSON.parse(localStorage.getItem("todos"))
 }

 $: localStorage.setItem("todos", JSON.stringify(todos))

  const addTodo = () => {
    if (!todo.texto.trim()) {
      console.log("texto vacio");
      todo.texto = "";
      return;
    }

    todo.id = Date.now();
    todos = [...todos, todo];
    console.log(todos);
    todo = { id: "", texto: "", estado: false };
  };

  const delTodo = (id) => {
    todos = todos.filter((item) => item.id !== id);
  };

  const updateTodos = (id) => {
    todos = todos.map((item) =>
      item.id === id ? { ...item, estado: !item.estado } : item
    );
  };

  const classIcono = valor => (
	  valor ? "bi bi-arrow-clockwise" : "bi bi-check2"
  )
  const classEstado = (valor) =>
  valor ? "btn btn-sm btn-success" : "btn btn-sm btn-warning";
</script>

<div class="container">
  <h1 class="my-3 display-6">CRUD</h1>
  <form on:submit|preventDefault={addTodo}>
    <input
      type="text"
      placeholder="Enter para agregar tarea"
      class="form-control shadow border-0"
      bind:value={todo.texto}
    />
  </form>
  {#each todos as item}
    <div class="shadow my-3 p-3 lead">
      <p class={item.estado ? "text-decoration-line-through" : ""}
	  >	{item.texto}</p>

      <button class="btn btn-sm {classEstado(item.estado)}" on:click={updateTodos(item.id)}>
        <i class={classIcono(item.estado)} /></button
      >

      <button class="btn btn-sm btn-danger" on:click={delTodo(item.id)}>
        <i class="bi bi-trash" />
      </button>
    </div>
  {/each}
</div>
