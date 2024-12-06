# Task-Management-Tool
#include <gtk/gtk.h>
#include "TaskManager.h"

TaskManager taskManager;

// Callback to add a task
void on_add_task_clicked(GtkWidget *widget, gpointer user_data) {
    GtkEntry *descEntry = GTK_ENTRY(gtk_builder_get_object(GTK_BUILDER(user_data), "entry_description"));
    GtkEntry *catEntry = GTK_ENTRY(gtk_builder_get_object(GTK_BUILDER(user_data), "entry_category"));
    GtkEntry *priEntry = GTK_ENTRY(gtk_builder_get_object(GTK_BUILDER(user_data), "entry_priority"));

    const char *description = gtk_entry_get_text(descEntry);
    const char *category = gtk_entry_get_text(catEntry);
    const char *priority = gtk_entry_get_text(priEntry);

    taskManager.addTask(description, category, priority);
    std::cout << "Task added: " << description << "\n";
}

// Callback to list tasks
void on_list_tasks_clicked(GtkWidget *widget, gpointer user_data) {
    taskManager.listTasks();
}

// Callback to delete a task
void on_delete_task_clicked(GtkWidget *widget, gpointer user_data) {
    GtkSpinButton *spinIndex = GTK_SPIN_BUTTON(gtk_builder_get_object(GTK_BUILDER(user_data), "spin_index"));
    int index = gtk_spin_button_get_value_as_int(spinIndex) - 1;

    taskManager.deleteTask(index);
    std::cout << "Task deleted at index: " << index + 1 << "\n";
}

// Main function
int main(int argc, char *argv[]) {
    GtkBuilder *builder;
    GtkWidget *window;

    gtk_init(&argc, &argv);

    builder = gtk_builder_new_from_file("task_manager.glade");
    window = GTK_WIDGET(gtk_builder_get_object(builder, "main_window"));

    gtk_builder_connect_signals(builder, builder);

    g_object_unref(builder);
    gtk_widget_show(window);
    gtk_main();

    return 0;
}
