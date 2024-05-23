<template>
  <div class="background-color">
    <v-img
      class="mx-auto my-6"
      max-width="200"
      src="../assets/logo.jpg"
    ></v-img>

    <v-card
      class="mx-auto pa-12 pb-8"
      elevation="8"
      max-width="418"
      rounded="lg"
    >
      <div class="text-subtitle-1 text-medium-emphasis">Login</div>

      <v-text-field
        density="compact"
        placeholder="Entre com o usuário"
        prepend-inner-icon="mdi-account-outline"
        variant="outlined"
      ></v-text-field>

      <div class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between">
        Senha
      </div>

      <v-text-field
        :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
        :type="visible ? 'text' : 'password'"
        density="compact"
        placeholder="Entre com a senha"
        prepend-inner-icon="mdi-lock-outline"
        variant="outlined"
        @click:append-inner="visible = !visible"
      ></v-text-field>

      <v-card
        class="mb-12"
        color="surface-variant"
        variant="tonal"
      >
        <v-card-text class="text-medium-emphasis text-caption">
          Sistema para gestão de estoque
        </v-card-text>
      </v-card>

      <v-btn
        class="mb-8"
        color="blue"
        size="large"
        variant="tonal"
        block
        to="/dashboard"
      >
        Log In
      </v-btn>
    </v-card>
  </div>
</template>

<script>
  export default {
    data: () => ({
      visible: false,
    }),
  }
</script>

