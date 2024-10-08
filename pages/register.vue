<script setup>
import { ref, computed } from 'vue';
import * as Yup from 'yup';

definePageMeta({
    title: 'Register',
    description: 'Register page description',
    layout: 'auth',
});

const { t } = useI18n()
const { showAlert } = useAlert()
const { register, loading, error, success } = useRegister();

const formSchema = ref({
    fields: [
        {
            label: t('common.name'),
            name: 'name',
            as: 'input',
            placeholder: t('common.name'),
            rules: Yup.string().min(3, t('validation.name.minLength')).required(t('validation.name.required')),
        },
        {
            label: t('common.email'),
            name: 'email',
            as: 'input',
            placeholder: '\u202Aexample@gmail.com',
            rules: Yup.string().matches(/^[\w\.-]+@[a-zA-Z\d\.-]+\.(com|net|org|edu|gov|mil|info|biz|co.uk|io)$/, t('validation.email.notValid')).required(t('validation.email.required')),
        },
        {
            label: t('common.phone'),
            name: 'phone',
            as: 'input',
            placeholder: '05-XXXX-XXXX', // 05-XXXXXXX
        },
        {
            label: t('common.password'),
            name: 'password',
            as: 'input',
            type: 'password',
            placeholder: '******',
            rules: Yup.string().min(6, t('validation.password.minLength')).required(t('validation.password.required')),
        },
    ],
});
const handleRegister = async (data) => {
    console.log(data)
    await register(data);

};
</script>

<template>
    <section class="h-full">
        <div class="flex min-h-full flex-1">
            <div class="flex flex-1 flex-col justify-center px-4 py-12 sm:px-6 lg:flex-none lg:px-20 xl:px-24">
                <div class="mx-auto w-full max-w-sm lg:w-96">
                    <div>
                        <NuxtLink to="/">
                            <IconsLogo class="h-14 w-auto " alt="zaman" />
                        </NuxtLink>
                        <h2 class="mt-8 text-2xl font-bold leading-9 tracking-tight text-gray-900">{{
                            $t('authentication.welcomeAgain')
                        }}</h2>
                        <div class="mt-2 text-sm leading-6 text-gray-500">
                            <div class="font-semibold text-gray-500">{{ $t("authentication.pleaseRegister") }}</div>
                        </div>
                    </div>

                    <div class="mt-10">
                        <div>
                            <DynamicForm :register="true" :schema="formSchema" @submit="handleRegister"
                                :serverError="error" class="space-y-6">
                                <template #buttuns>
                                    <div>
                                        <button type="submit"
                                            :class="{ 'cursor-not-allowed': loading, 'opacity-50': loading }"
                                            :disabled="loading"
                                            class="flex w-full justify-center rounded-md bg-zaman-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-zaman-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-zaman-600">

                                            <span v-if="!loading">
                                                {{ $t('authentication.register') }}
                                            </span>
                                            <span v-if="loading">
                                                {{ $t('loading') }}
                                                <IconsLoadingWhite />
                                            </span>
                                        </button>
                                    </div>
                                </template>
                            </DynamicForm>
                        </div>

                        <div class="mt-10">
                            <div class="relative">
                                <div class="absolute inset-0 flex items-center" aria-hidden="true">
                                    <div class="w-full border-t border-gray-200" />
                                </div>
                                <div class="relative flex justify-center text-sm font-medium leading-6">
                                    <span class="bg-white px-6 text-gray-900">{{ $t('authentication.or') }}</span>
                                </div>
                            </div>

                            <div class="mt-6 grid grid-cols-1 gap-4">
                                <a href="#"
                                    class="flex w-full items-center justify-center gap-3 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus-visible:ring-transparent">
                                    <svg class="h-5 w-5" aria-hidden="true" viewBox="0 0 24 24">
                                        <path
                                            d="M12.0003 4.75C13.7703 4.75 15.3553 5.36002 16.6053 6.54998L20.0303 3.125C17.9502 1.19 15.2353 0 12.0003 0C7.31028 0 3.25527 2.69 1.28027 6.60998L5.27028 9.70498C6.21525 6.86002 8.87028 4.75 12.0003 4.75Z"
                                            fill="#EA4335" />
                                        <path
                                            d="M23.49 12.275C23.49 11.49 23.415 10.73 23.3 10H12V14.51H18.47C18.18 15.99 17.34 17.25 16.08 18.1L19.945 21.1C22.2 19.01 23.49 15.92 23.49 12.275Z"
                                            fill="#4285F4" />
                                        <path
                                            d="M5.26498 14.2949C5.02498 13.5699 4.88501 12.7999 4.88501 11.9999C4.88501 11.1999 5.01998 10.4299 5.26498 9.7049L1.275 6.60986C0.46 8.22986 0 10.0599 0 11.9999C0 13.9399 0.46 15.7699 1.28 17.3899L5.26498 14.2949Z"
                                            fill="#FBBC05" />
                                        <path
                                            d="M12.0004 24.0001C15.2404 24.0001 17.9654 22.935 19.9454 21.095L16.0804 18.095C15.0054 18.82 13.6204 19.245 12.0004 19.245C8.8704 19.245 6.21537 17.135 5.2654 14.29L1.27539 17.385C3.25539 21.31 7.3104 24.0001 12.0004 24.0001Z"
                                            fill="#34A853" />
                                    </svg>
                                    <span class="text-sm font-semibold leading-6">Google</span>
                                </a>
                            </div>

                            <p class="text-center text-sm leading-6 text-gray-500 py-4">
                                {{ $t('authentication.doHaveAccount') }}{{ ' ' }}
                                <NuxtLink to="/login" class="font-semibold text-zaman hover:text-zaman-700">{{
                                    $t('authentication.login') }}</NuxtLink>
                            </p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="relative hidden w-0 flex-1 lg:block">
                <img class="absolute inset-0 h-full w-full object-cover" src="@/assets/img/register.png" alt="" />
            </div>
        </div>
    </section>
</template>