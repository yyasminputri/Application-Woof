**Application-Woof**
Aplikasi Android modern yang menampilkan anjing-anjing menggemaskan dengan desain UI pink lembut yang elegan, dibangun menggunakan Jetpack Compose.

## ✨ Features
- 🎨 Soft Pink Theme - UI gradien cantik dengan palet warna pink dan lavender
- 💳 Interactive Cards - Kartu anjing yang dapat diperluas dengan animasi halus
- ❤️ Favorite System - Tandai anjing favoritmu dengan ikon hati
- 📱 Modern Design - Desain Material 3 dengan sudut membulat dan bayangan
- 🌙 Dark Mode - Dukungan tema gelap yang elegan
- 📊 Dog Stats - Lencana usia dan indikator tingkat energi
- 🔄 Smooth Animations - Animasi buka/tutup yang menyenangkan dengan fisika pegas

## 🛠️ Tech Stack
- Kotlin - 100% Kotlin codebase
- Jetpack Compose - Modern declarative UI toolkit
- Material 3 - Latest Material Design components
- Custom Theming - Soft pink color scheme with light/dark variants
- Animation API - Spring-based animations for smooth interactions

## 📝 Project Structure
app/src/main/java/com/example/woof/
├── MainActivity.kt          # Main activity with enhanced UI
├── data/
│   └── Dog.kt              # Dog data class and sample data
└── ui/theme/
    ├── Color.kt            # Soft pink color palette
    ├── Shape.kt            # Rounded corner shapes
    ├── Theme.kt            # Material 3 theming
    └── Type.kt             # Typography definitions

## 🛠️ Langkah Pembuatan Aplikasi WOOF
1. Buat Project Android dengan Jetpack Compose
- Buka Android Studio → New Project → Pilih Empty Compose Activity.
- Beri nama proyek: Woof.
- Pastikan menggunakan Kotlin dan Jetpack Compose.

2. Siapkan File-File
- Data/Dog.kt
```
package com.example.woof.data

import androidx.annotation.DrawableRes
import androidx.annotation.StringRes
import com.example.woof.R

data class Dog(
    @DrawableRes val imageResourceId: Int,
    @StringRes val name: Int,
    val age: Int,
    @StringRes val hobbies: Int
)

val dogs = listOf(
    Dog(R.drawable.luna, R.string.dog_name_1, 3, R.string.dog_description_1),
    Dog(R.drawable.jake, R.string.dog_name_2, 2, R.string.dog_description_2),
    Dog(R.drawable.lucy, R.string.dog_name_3, 1, R.string.dog_description_3),
    Dog(R.drawable.milo, R.string.dog_name_4, 2, R.string.dog_description_4),
    Dog(R.drawable.coco, R.string.dog_name_5, 5, R.string.dog_description_5),
    Dog(R.drawable.daisy, R.string.dog_name_6, 6, R.string.dog_description_6),
    Dog(R.drawable.rocky, R.string.dog_name_7, 4, R.string.dog_description_7),
    Dog(R.drawable.churro, R.string.dog_name_8, 3, R.string.dog_description_8),
    Dog(R.drawable.waffles, R.string.dog_name_9, 3, R.string.dog_description_9)
```
- MainActivity.kt
```
package com.example.woof

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.annotation.DrawableRes
import androidx.compose.animation.animateContentSize
import androidx.compose.animation.core.Spring
import androidx.compose.animation.core.spring
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ExpandLess
import androidx.compose.material.icons.filled.ExpandMore
import androidx.compose.material.icons.filled.Favorite
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.draw.shadow
import androidx.compose.ui.graphics.Brush
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.example.woof.data.Dog
import com.example.woof.data.dogs
import com.example.woof.ui.theme.WoofTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            WoofTheme {
                Surface(
                    modifier = Modifier.fillMaxSize()
                ) {
                    WoofApp()
                }
            }
        }
    }
}

@Composable
fun WoofApp() {
    Scaffold(
        topBar = {
            WoofTopAppBar()
        }
    ) { it ->
        // Background gradient
        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(
                    brush = Brush.verticalGradient(
                        colors = listOf(
                            MaterialTheme.colorScheme.primaryContainer.copy(alpha = 0.3f),
                            MaterialTheme.colorScheme.surface
                        )
                    )
                )
        ) {
            LazyColumn(
                contentPadding = PaddingValues(
                    top = it.calculateTopPadding() + 8.dp,
                    start = 16.dp,
                    end = 16.dp,
                    bottom = 16.dp
                ),
                verticalArrangement = Arrangement.spacedBy(12.dp)
            ) {
                items(dogs) { dog ->
                    DogItem(
                        dog = dog,
                        modifier = Modifier.fillMaxWidth()
                    )
                }
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun DogItem(
    dog: Dog,
    modifier: Modifier = Modifier
) {
    var expanded by remember { mutableStateOf(false) }
    var isFavorite by remember { mutableStateOf(false) }

    Card(
        modifier = modifier
            .shadow(
                elevation = 8.dp,
                shape = MaterialTheme.shapes.large
            )
            .animateContentSize(
                animationSpec = spring(
                    dampingRatio = Spring.DampingRatioMediumBouncy,
                    stiffness = Spring.StiffnessLow
                )
            ),
        shape = MaterialTheme.shapes.large,
        colors = CardDefaults.cardColors(
            containerColor = MaterialTheme.colorScheme.surface
        ),
        onClick = { expanded = !expanded }
    ) {
        Column {
            // Main content row
            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(16.dp),
                verticalAlignment = Alignment.CenterVertically
            ) {
                // Dog image with circular shape
                Box {
                    DogIcon(
                        dogIcon = dog.imageResourceId,
                        modifier = Modifier
                            .size(80.dp)
                            .clip(CircleShape)
                    )
                    // Age badge
                    Surface(
                        modifier = Modifier
                            .align(Alignment.BottomEnd)
                            .offset(x = (-4).dp, y = (-4).dp),
                        shape = CircleShape,
                        color = MaterialTheme.colorScheme.primary
                    ) {
                        Text(
                            text = "${dog.age}",
                            modifier = Modifier.padding(6.dp),
                            style = MaterialTheme.typography.labelSmall,
                            color = MaterialTheme.colorScheme.onPrimary,
                            fontWeight = FontWeight.Bold
                        )
                    }
                }

                Spacer(modifier = Modifier.width(16.dp))

                // Dog information
                Column(
                    modifier = Modifier.weight(1f)
                ) {
                    Text(
                        text = stringResource(dog.name),
                        style = MaterialTheme.typography.headlineSmall,
                        fontWeight = FontWeight.Bold,
                        color = MaterialTheme.colorScheme.onSurface
                    )
                    Text(
                        text = stringResource(R.string.years_old, dog.age),
                        style = MaterialTheme.typography.bodyMedium,
                        color = MaterialTheme.colorScheme.onSurfaceVariant
                    )
                }

                // Favorite button
                IconButton(
                    onClick = { isFavorite = !isFavorite }
                ) {
                    Icon(
                        imageVector = Icons.Filled.Favorite,
                        contentDescription = "Favorite",
                        tint = if (isFavorite) MaterialTheme.colorScheme.primary
                        else MaterialTheme.colorScheme.outline
                    )
                }

                // Expand button
                IconButton(
                    onClick = { expanded = !expanded }
                ) {
                    Icon(
                        imageVector = if (expanded) Icons.Filled.ExpandLess
                        else Icons.Filled.ExpandMore,
                        contentDescription = if (expanded) "Show less" else "Show more",
                        tint = MaterialTheme.colorScheme.primary
                    )
                }
            }

            // Expanded content
            if (expanded) {
                Divider(
                    modifier = Modifier.padding(horizontal = 16.dp),
                    color = MaterialTheme.colorScheme.outline.copy(alpha = 0.3f)
                )

                Column(
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(
                        text = "About ${stringResource(dog.name)}",
                        style = MaterialTheme.typography.titleMedium,
                        fontWeight = FontWeight.Bold,
                        color = MaterialTheme.colorScheme.primary
                    )

                    Spacer(modifier = Modifier.height(8.dp))

                    Text(
                        text = stringResource(dog.hobbies),
                        style = MaterialTheme.typography.bodyMedium,
                        color = MaterialTheme.colorScheme.onSurfaceVariant,
                        lineHeight = MaterialTheme.typography.bodyMedium.lineHeight * 1.2
                    )

                    Spacer(modifier = Modifier.height(12.dp))

                    // Fun facts row
                    Row(
                        modifier = Modifier.fillMaxWidth(),
                        horizontalArrangement = Arrangement.SpaceEvenly
                    ) {
                        InfoChip(
                            label = "Age",
                            value = "${dog.age} years"
                        )
                        InfoChip(
                            label = "Status",
                            value = "Good Dog"
                        )
                        InfoChip(
                            label = "Energy",
                            value = if (dog.age < 3) "High" else "Medium"
                        )
                    }
                }
            }
        }
    }
}

@Composable
fun InfoChip(
    label: String,
    value: String,
    modifier: Modifier = Modifier
) {
    Surface(
        modifier = modifier,
        shape = MaterialTheme.shapes.small,
        color = MaterialTheme.colorScheme.primaryContainer.copy(alpha = 0.7f)
    ) {
        Column(
            modifier = Modifier.padding(horizontal = 12.dp, vertical = 8.dp),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Text(
                text = label,
                style = MaterialTheme.typography.labelSmall,
                color = MaterialTheme.colorScheme.onPrimaryContainer,
                fontWeight = FontWeight.Bold
            )
            Text(
                text = value,
                style = MaterialTheme.typography.bodySmall,
                color = MaterialTheme.colorScheme.onPrimaryContainer
            )
        }
    }
}

@Composable
fun WoofTopAppBar(modifier: Modifier = Modifier) {
    CenterAlignedTopAppBar(
        title = {
            Row(
                verticalAlignment = Alignment.CenterVertically
            ) {
                Image(
                    modifier = Modifier
                        .size(48.dp)
                        .padding(4.dp),
                    painter = painterResource(R.drawable.ic_woof_logo),
                    contentDescription = null
                )
                Text(
                    text = stringResource(R.string.app_name),
                    style = MaterialTheme.typography.headlineMedium,
                    fontWeight = FontWeight.Bold
                )
            }
        },
        colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
            containerColor = MaterialTheme.colorScheme.primaryContainer.copy(alpha = 0.8f),
            titleContentColor = MaterialTheme.colorScheme.onPrimaryContainer
        ),
        modifier = modifier
    )
}

@Composable
fun DogIcon(
    @DrawableRes dogIcon: Int,
    modifier: Modifier = Modifier
) {
    Image(
        modifier = modifier,
        contentScale = ContentScale.Crop,
        painter = painterResource(dogIcon),
        contentDescription = null
    )
}

@Preview
@Composable
fun WoofPreview() {
    WoofTheme(darkTheme = false) {
        WoofApp()
    }
}

@Preview
@Composable
fun WoofDarkThemePreview() {
    WoofTheme(darkTheme = true) {
        WoofApp()
    }
}
```

## 📱 Screenshots
<img width="165" alt="Screenshot 2025-05-22 at 21 47 34" src="https://github.com/user-attachments/assets/fa8478a1-5595-4c1d-b91d-6a43efb627f8" />
<img width="174" alt="Screenshot 2025-05-22 at 21 48 00" src="https://github.com/user-attachments/assets/cec1a8b6-3a9a-415a-a970-96db9cfe68bb" />
