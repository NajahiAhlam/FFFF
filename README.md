getFournisseurName(fournisseurId: number): Observable<string> {
  return this.gestionFournisseurService.getFournisseurName(fournisseurId).pipe(
    map((response: FournisseurResponse) => response.nom),
    catchError(error => {
      console.error('Error fetching fournisseur name:', error);
      return of('Unknown');
    })
  );
}
