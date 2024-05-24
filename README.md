getFournisseurName(fournisseurId: number): Observable<string> {
  return this.gestionFournisseurService.getFournisseurName(fournisseurId).pipe(
    map(response => response.nom || 'Unknown'), // Assuming response has a 'nom' property
    catchError(error => {
      console.error('Error fetching fournisseur name:', error);
      return of('Unknown');
    })
  );
}
