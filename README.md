#include <gtk/gtk.h>

// Callback pour gérer la sélection de date
void on_date_selected(GtkCalendar *calendar, GtkLabel *label) {
    guint year, month, day;

    // Récupérer la date sélectionnée
    gtk_calendar_get_date(calendar, &year, &month, &day);

    // GTK utilise des mois de 0 (janvier) à 11 (décembre), donc on ajoute 1
    char date_str[50];
    snprintf(date_str, sizeof(date_str), "Date sélectionnée : %02d/%02d/%04d", day, month + 1, year);

    // Mettre à jour l'étiquette avec la date sélectionnée
    gtk_label_set_text(label, date_str);
}

int main(int argc, char *argv[]) {
    GtkWidget *window;
    GtkWidget *vbox;
    GtkWidget *calendar;
    GtkWidget *label;

    // Initialiser GTK
    gtk_init(&argc, &argv);

    // Créer la fenêtre principale
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Calendrier GTK3");
    gtk_window_set_default_size(GTK_WINDOW(window), 300, 200);
    gtk_container_set_border_width(GTK_CONTAINER(window), 10);

    // Connecter le signal pour fermer l'application
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);

    // Créer une boîte verticale pour organiser les widgets
    vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 10);
    gtk_container_add(GTK_CONTAINER(window), vbox);

    // Ajouter le widget calendrier
    calendar = gtk_calendar_new();
    gtk_box_pack_start(GTK_BOX(vbox), calendar, TRUE, TRUE, 0);

    // Ajouter une étiquette pour afficher la date sélectionnée
    label = gtk_label_new("Sélectionnez une date.");
    gtk_box_pack_start(GTK_BOX(vbox), label, FALSE, FALSE, 0);

    // Connecter le signal de sélection de date au calendrier
    g_signal_connect(calendar, "day-selected", G_CALLBACK(on_date_selected), label);

    // Afficher tous les widgets
    gtk_widget_show_all(window);

    // Lancer la boucle principale GTK
    gtk_main();

    return 0;
}
