<template>
  <content-placeholders class="relative flex rounded py-2 px-3 cursor-pointer flex-col sm:flex-row sm:mb-0 mb-4" :rounded="true">
    <div class="w-full mr-0 sm:w-auto sm:mr-6 sm:block hidden">
      <div class="relative w-12-5 h-12-5 inline-block">
        <content-placeholders-img class="w-full h-full rounded-full" style="height: 100%; border-radius: 100%" />
      </div>
    </div>
    <div class="w-full sm:w-5/6">
      <content-placeholders-text class="m-0 mb-3" :lines="1" />
      <div class="h-4">
        <content-placeholders-text :lines="1" class="inline-block" style="width: 5rem" />
        <content-placeholders-text :lines="1" class="inline-block" style="width: 5rem" />
      </div>
    </div>
    <div class="flex items-center justify-between flex-row-reverse my-2 sm:my-0">
        <div class="block ml-4 w-24">
        <content-placeholders-text :lines="1" />
        </div>
        <div class="flex items-center justify-center ml-4" style="height: 15px">
          <content-placeholders-text style="width: 3rem" :lines="1" />
        </div>
    </div>
  </content-placeholders>
<!--   <div class="relative flex rounded py-2 px-3 transition-all duration-200 ease-in-out cursor-pointer flex-col sm:flex-row sm:border-none sm:mb-0 border border-solid border-gray-200 mb-4">
    <div class="w-full mr-0 sm:w-auto sm:mr-6 sm:block hidden">
      <div class="relative w-12-5 h-12-5 inline-block">
        <div class="w-full h-full rounded-full object-cover bg-gray-300"></div>
      </div>
    </div>
    <div class="w-full sm:w-5/6">
      <h3 class="m-0 mb-3">
        <span style="width:65%;" class="block h-4 rounded bg-gray-300"></span>
      </h3>
      <div class="h-4">
        <span style="width: 5rem" class="inline-block h-full bg-gray-300 rounded mr-2">
        </span>
        <span style="width: 5rem" class="inline-block h-full bg-gray-300 rounded"></span>
      </div>
    </div>
    <div class="flex items-center justify-between flex-row-reverse mt-2 sm:mt-0">
      <span class="block bg-gray-300 ml-4 w-24 rounded"><span style="visibility: hidden;">h</span></span>
      <div class="flex items-center justify-center ml-4" style="height: 15px">
        <span style="width: 3rem" class="inline-block h-full bg-gray-300 rounded"></span>
      </div>
      <div class="flex items-center justify-center ml-4" style="height: 15px">
        <span style="width: 3rem" class="inline-block h-full bg-gray-300 rounded"></span>
      </div>
    </div>
  </div> -->
</template>