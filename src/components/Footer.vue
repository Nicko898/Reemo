<script>
export default {
  name: 'Footer'
}
</script>

<template>
  <footer class="bg-white shadow bg-zinc-900">
    <div class="w-full max-w-screen-xl mx-auto p-4 md:py-8">
      <div class="sm:flex sm:items-center sm:justify-between">
        <a href="https://flowbite.com/" class="flex items-center mb-4 sm:mb-0 space-x-3 rtl:space-x-reverse">
          <img src="https://flowbite.com/docs/images/logo.svg" class="h-8" alt="Flowbite Logo" />
          <span class="self-center text-white text-2xl font-semibold whitespace-nowrap ">Reemo</span>
        </a>
        <ul class="flex flex-wrap items-center mb-6 text-sm font-medium text-gray-500 sm:mb-0 ">
          <li>
            <a class="hover:underline me-4 md:me-6">Sobre nosotros</a>
          </li>
          <li>
            <a class="hover:underline me-4 md:me-6">Políticas de Seguridad</a>
          </li>
          <li>
            <a class="hover:underline me-4 md:me-6">Licencias</a>
          </li>
          <li>
            <a class="hover:underline">Contactos</a>
          </li>
        </ul>
      </div>
      <hr class="my-6 border-gray-200 sm:mx-auto  lg:my-8" />
      <span class="block text-sm text-gray-500 sm:text-center ">
        © 2023 Reemo. Todos los derechos reservados.
      </span>
    </div>
  </footer>
</template>