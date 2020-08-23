<template>
    <div class="px-5 flex flex-col w-11/12">
        <div class="flex flex-rows text-lg py-8 relative">
            <h1>Hasil pengecekan</h1>
            <h1 v-show="dataCost" class="right-0 absolute text-base font-normal"><b>{{ getCity(dataCost['origin']) }}</b> ke <b>{{ getCity(dataCost['destination']) }}</b> @<b>{{dataCost.weight  }}</b> gram</h1>
        </div>

        <div class="font-semibold flex flex-rows text-xs font-extrabold">
            <h1 class=""><i class="fa fa-filter" aria-hidden="true"></i> Pilih kurir</h1>

            <div class="flex ml-5">
                <label class="flex items-center">
                    <input type="checkbox" class="cursor-pointer form-checkbox h-3" checked>
                    <span class="cursor-pointer ml-1">JNE</span>
                </label>
            </div>
            
            <div class="flex mx-5">
                <label class="flex items-center">
                    <input type="checkbox" class="cursor-pointer form-checkbox h-3">
                    <span class="cursor-pointer ml-1">TIKI</span>
                </label>
            </div>

            <div class="flex">
                <label class="flex items-center">
                    <input type="checkbox" class="cursor-pointer form-checkbox h-3">
                    <span class="cursor-pointer ml-1">POS</span>
                </label>
            </div>
        </div>

        <div class="flex flex-rows px-3 relative pt-5 pb-3">
            <h1 class="pr-40">Kurir</h1>
            <h1 class="">Jenis Layanan</h1>
            <h1 class=" absolute right-0 pr-3 cursor-pointer">
                <i class="fa fa-sort-up" aria-hidden="true" style="color:orange;"></i>
                <!-- <i class="fa fa-sort-down" aria-hidden="true"></i> -->
                Tarif</h1>
        </div>
        <hr class="border border-solid border-teal-500">

        <div class="flex flex-col my-2">
            <div v-for="items in dataCost.results" :key="items.index" class="mb-2 border border-l-4 border-solid border-gray-400 px-4 bg-gray-100">
                <div class="flex flex-rows py-2">
                    <div class="flex-1 flex flex-col">
                        <h1 class="text-xl font-bold">{{items.code.toUpperCase()}}</h1>
                        <span class="text-xs pt-1">{{ items.name }}</span>
                    </div>
                    <div class="flex-1 flex flex-col">
                        <h1 class="text-xl font-bold">{{items.service}}</h1>
                        <span class="text-xs pt-1">{{ items.description }}</span>
                    </div>
                    <div class="font-bold text-xl text-teal-500">
                        <h1>Rp {{items.costAmount}}</h1>
                    </div>
                </div>
            </div>
        </div>
    </div>  
</template>

<script>
export default {
    name : 'Container',
    props: ['dataCost','City'],
    methods : {
        getCity(id) {
            if(id) {
                const items = this.City.find(el => el.city_id === id);
                return items.city_name;
                
            }else{
                return ' ';
            }
        },
    }
}
</script>

<style scoped>

</style>