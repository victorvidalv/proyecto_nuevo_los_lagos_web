<script>
  // Importa Axios si aún no lo has hecho
  import axios from "axios";

  // Array para almacenar los datos del formulario
  let inputData = {
    name: "",
    email: "",
    phone: "",
    type: "emprendedor",
    city: "",
    company: "",
    rutcompany: "",
    websitecompany: "",
    career: "",
    position: "",
    institution: "",
    businessIdea: "",
  };

  let studystate = "";

  //Funcion para pasar de un paso a otro
  let step = -2;

  let send = 0;

  function nextStep() {
    step++;
  }

  // Función para enviar el formulario
  async function sendForm() {
    //contatena el estado de estudiante

    if (inputData.type === "estudiante") {
      inputData.type = inputData.type + " " + studystate;
    }

    send = 1;
    try {
      const response = await axios.post(
        "https://www.t2g.cl/apis/public/api/preregistration",
        inputData,
        {
          headers: {
            "Content-Type": "application/json",
          },
        }
      );

      if (response.status === 200 || response.status === 201) {
        alert(
          "Preinscripción enviada correctamente, pronto nos contactaremos con usted."
        );
        send = 2;
      } else {
        showError();
      }
    } catch (error) {
      showError();
    }
  }

  // Función para mostrar un mensaje de error al usuario
  function showError() {
    alert(
      "Ocurrió un error al enviar el formulario, verifique los datos e intente nuevamente."
    );
    send = 4;
  }

  let comunas = [
    "Ancud",
    "Castro",
    "Chonchi",
    "Curaco de Vélez",
    "Dalcahue",
    "Puqueldón",
    "Queilén",
    "Quemchi",
    "Quellón",
    "Quinchao",
    "Calbuco",
    "Cochamó",
    "Fresia",
    "Frutillar",
    "Llanquihue",
    "Los Muermos",
    "Maullín",
    "Puerto Montt",
    "Puerto Varas",
    "Osorno",
    "Puerto Octay",
    "Purranque",
    "Puyehue",
    "Río Negro",
    "San Juan de la Costa",
    "San Pablo",
    "Chaitén",
    "Futaleufú",
    "Hualaihué",
    "Palena",
    "Otra",
  ];
</script>

<!-- PAGE START -->

<div class="w-full max-w-xl xl:px-8 xl:w-5/12">
  <div class="bg-white rounded shadow-2xl p-7 sm:p-10">
    <h3 class="mb-4 text-xl font-semibold sm:text-center sm:mb-6 sm:text-2xl">
     Etapa de Selección
    </h3>
    <form onsubmit="event.preventDefault(); sendForm()">
      <!-- Step 0 -->

      {#if step === -2}
        <div>
          <div class="dividers">
            <p class="mt-5">
              1. Para participar debe ser mayor de 18 años. En caso de empresas
              debe tener ventas inferiores a 24.000 UF y antigüedad menor a 36
              meses.
            </p>
            <p class="mt-5">
              2. Debe tener domicilio en la <strong>Región de Los Lagos</strong
              >.
            </p>

            <p class="mt-5">
              3. Debe contar con una idea de negocio o con un negocio en
              funcionamiento, cuya ocupación esté relacionada con oportunidades
              de MA e I4.0.
            </p>
          </div>

          <div class="buttondiv">
            <button on:click={nextStep}>Comenzar</button>
          </div>
        </div>
      {/if}

      <!-- Step 0 -->

      {#if step === -1}
        <div>
          <p class="mt-5">
            Al realizar esta preinscripción, estás dando el primer paso hacia
            una emocionante oportunidad.
          </p>
          <p />
          <p class="mt-5"></p>
          Una vez que tu preinscripción sea revisada y aceptada, nos pondremos
          en contacto contigo para confirmar tu participación en el programa.
          <p />

          <p class="mt-5 text-sm text-blue-900">
            * En adelante el botón continuar aparecerá una vez que todos los
            campos requeridos estén completos.
          </p>
          <p />

          <div class="buttondiv">
            <button on:click={nextStep}>Continuar</button>
          </div>
        </div>
      {/if}

      <!-- Step 0 -->

      {#if step === 0}
        <div>
          <div class="dividers">
            <label>Nombre</label>
            <input
              bind:value={inputData.name}
              placeholder="Nombre completo"
              type="text"
              name="name"
            />
          </div>

          <div class="dividers">
            <label>Email</label>
            <input
              bind:value={inputData.email}
              placeholder="email@ejemplo.cl"
              type="email"
              name="email"
            />
          </div>

          <div class="dividers">
            <label>Teléfono</label>
            <input
              bind:value={inputData.phone}
              placeholder="+569 99 99 999"
              type="text"
              name="phone"
            />
          </div>

          {#if inputData.name && inputData.email && inputData.phone}
            <div class="buttondiv">
              <button on:click={nextStep}>Continuar</button>
            </div>
          {:else}{/if}
        </div>
      {/if}

      <!-- Step 1 -->

      {#if step === 1}
        <div>
          <div class="dividers">
            <label>Comuna</label>
            <select bind:value={inputData.city} name="city">
              {#each comunas as comuna}
                <option value={comuna}>{comuna}</option>
              {/each}
            </select>
          </div>

          <div class="dividers">
            <label>Tipo de registro</label>
            <select bind:value={inputData.type} name="type">
              <option value="emprendedor">Emprendedor</option>
              <option value="trabajador">Trabajador</option>
              <option value="estudiante">Estudiante</option>
            </select>
          </div>

          {#if inputData.city && inputData.type}
            <div class="buttondiv">
              <button on:click={nextStep}>Continuar</button>
            </div>
          {/if}
        </div>
      {/if}

      <!-- Step 2 -->

      {#if step === 2 && inputData.type === "emprendedor"}
        <div>
          <div class="dividers">
            <label>Empresa / Emprendimiento</label>
            <input
              bind:value={inputData.company}
              placeholder="Nombre Empresa o emprendimiento"
              type="text"
              name="company"
            />
          </div>

          <div class="dividers">
            <label>Rut Empresa</label>
            <input
              bind:value={inputData.rutcompany}
              placeholder="77.777.777-k"
              type="text"
              name="rutcompany"
            />
          </div>

          <div class="dividers">
            <label>Sitio Web</label>
            <input
              bind:value={inputData.websitecompany}
              placeholder="ejemplo.com"
              type="text"
              name="websitecompany"
            />
          </div>

          {#if inputData.company && inputData.rutcompany && inputData.websitecompany}
            <div class="buttondiv">
              <button on:click={nextStep}>Continuar</button>
            </div>
          {/if}

          <p class="my-3">
            Si aún no tiene Rut o sitio web, escriba <strong>ninguno</strong>
          </p>
        </div>
      {/if}

      <!-- Step 2 -->

      {#if step === 2 && inputData.type === "trabajador"}
        <div>
          <div class="dividers">
            <label>Empresa</label>
            <input
              bind:value={inputData.company}
              placeholder="Nombre Empresa"
              type="text"
              name="company"
            />
          </div>

          <div class="dividers">
            <label>Rut Empresa</label>
            <input
              bind:value={inputData.rutcompany}
              placeholder="77.777.777-k"
              type="text"
              name="rutcompany"
            />
          </div>

          <div class="dividers">
            <label> Cargo</label>
            <input
              bind:value={inputData.position}
              placeholder="Cargo"
              type="text"
              name="position"
            />
          </div>

          <div class="dividers">
            <label>Sitio Web</label>
            <input
              bind:value={inputData.websitecompany}
              placeholder="ejemplo.com"
              type="text"
              name="websitecompany"
            />
          </div>

          {#if inputData.company && inputData.rutcompany && inputData.position && inputData.websitecompany}
            <div class="buttondiv">
              <button on:click={nextStep}>Continuar</button>
            </div>
          {/if}
        </div>
      {/if}

      <!-- Step 2 -->

      {#if step === 2 && inputData.type === "estudiante"}
        <div>
          <div class="dividers">
            <label>Carrera</label>
            <input
              bind:value={inputData.career}
              placeholder="Carrera"
              type="text"
              name=" career"
            />
          </div>

          <div class="dividers">
            <label>Institución Educativa</label>
            <input
              bind:value={inputData.institution}
              placeholder="Institución Educativa"
              type="text"
              name="institution"
            />
          </div>

          <div class="dividers">
            <label>Estado</label>
            <select bind:value={studystate} name="studystate">
              <option value="cursando menos de 3 años"
                >Cursando menos de 3 años</option
              >
              <option value="cursando más de 3 años"
                >Cursando más de 3 años</option
              >
              <option value="egresado">Egresado</option>
              <option value="titulado">Titulado</option>
            </select>
          </div>

          {#if inputData.career && inputData.institution}
            <div class="buttondiv">
              <button on:click={nextStep}>Continuar</button>
            </div>
          {/if}
        </div>
      {/if}

      <!-- Step 3 -->

      {#if step === 3}
        <div>
          <div class="dividers">
            <label>Describa muy brevemente el proyecto o idea de negocio</label>
            <textarea
              bind:value={inputData.businessIdea}
              placeholder="Ejemplo: Desarrollo de una app que por medio de la IA puede predecir el fallas."
              name="businessIdea"
            ></textarea>
          </div>

          {#if inputData.businessIdea}
            <div class="buttondiv">
              <button on:click={nextStep}>Continuar</button>
            </div>
          {/if}
        </div>
      {/if}

      <!-- Step 4 -->

      {#if step === 4}
        <div>
          <div>
            <p class="mb-3">
              <strong
                >Se han registrado lo siguientes datos de contacto:<strong
                ></strong></strong
              >
            </p>
            <p>Nombre: {inputData.name}</p>
            <p>Email: {inputData.email}</p>
            <p>Telefono: {inputData.phone}</p>
            <p>Tipo de preinscripción: {inputData.type}</p>
          </div>

          <div>
            <p class="mt-3">
              Si son correctos, presione enviar para finalizar el preinscripción
            </p>
          </div>

          {#if send === 0}
            <div class="buttondiv">
              <button on:click={sendForm}> Enviar Preinscripción </button>
            </div>
          {/if}

          {#if send === 1}
            <div class="buttondiv">
              <button disabled> Enviando ... </button>
            </div>
          {/if}

          {#if send === 2}
            <div class="buttondiv">
              <button disabled> Enviado </button>
            </div>
          {/if}

          {#if send === 4}
            <div class="buttondiv">
              <button on:click={sendForm}> Reintentar </button>
            </div>
          {/if}
        </div>
      {/if}

      <p class="text-xs text-gray-600 sm:text-sm">
        Por favor complete todos los campos requeridos para continuar.
      </p>

      {#if step > 0}
        <span class="reset" on:click={() => (step = -2)}>inicio</span>
      {/if}
    </form>
  </div>
</div>

<!-- PAGE END -->

<!-- Styles -->

<style lang="sass">

  input[type="text"], input[type="email"], select, textarea
    @apply flex-grow w-full h-12 px-4 mb-2 bg-white border border-gray-300 appearance-none

  textarea
    @apply h-32

  label
    @apply inline-block mb-1 font-medium

  button
    @apply inline-flex items-center justify-center w-full h-12 px-6 font-medium tracking-wide text-white transition duration-200 rounded shadow-md bg-deep-purple-accent-400 hover:bg-deep-purple-accent-700 

  .reset 
    @apply cursor-pointer mt-3 px-2 bg-green-500 text-white rounded

  .dividers 
    @apply mb-1 sm:mb-2

  .buttondiv 
    @apply mt-4 mb-2 sm:mb-4

</style>
